##!/bin/bash

## Set variables
NEW_CONTAINER_NAME="rediscover_new"

FINAL_CONTAINER_NAME="rediscover"

TEMP_PORT=3001  # Temporary port for the new container

FINAL_PORT=3000 # Final port for the production container

TEMP_IMAGE_NAME="rediscover_temp"

IMAGE_NAME="rediscover"

## Build new Docker image without cache
docker build -t ${TEMP_IMAGE_NAME} .

## Stop and remove any existing container with the final name
if docker ps -a --format '{{.Names}}' | grep -q "^${FINAL_CONTAINER_NAME}$"; then

  docker stop ${FINAL_CONTAINER_NAME} || true

  docker rm -f ${FINAL_CONTAINER_NAME} || true

fi

## Run new container with a unique name on the temporary port
docker run -d --name ${NEW_CONTAINER_NAME} -p ${TEMP_PORT}:80 ${TEMP_IMAGE_NAME}

## Wait for the new container to be ready
until curl -s http://localhost:${TEMP_PORT} > /dev/null; do

  sleep 1

done

## Stop and remove the current production container if it's running
if docker ps --filter "name=${FINAL_CONTAINER_NAME}" --filter "status=running" | grep -q .; then

  docker stop ${FINAL_CONTAINER_NAME} || true

  docker rm ${FINAL_CONTAINER_NAME} || true

fi

## Rename the new container to the final name
docker rename ${NEW_CONTAINER_NAME} ${FINAL_CONTAINER_NAME}

## Update the port binding
docker stop ${FINAL_CONTAINER_NAME} || true

docker rm ${FINAL_CONTAINER_NAME} || true

docker run -d --name ${FINAL_CONTAINER_NAME} -p ${FINAL_PORT}:80 ${TEMP_IMAGE_NAME}

## Optionally, keep specific images by excluding them from prune
### For example, keep the 'rediscover' image to speed up future builds
docker image prune -f --filter "label!=rediscover"

## Clean up stopped containers and unused networks
docker container prune -f
docker network prune -f

echo "Rolling update completed successfully"

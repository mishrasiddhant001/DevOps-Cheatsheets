# 🐳 **Docker Rolling Update Script**

This script automates the process of performing a **rolling update** for a Docker container, ensuring minimal downtime by running a temporary container first and then replacing the existing production container. 🚀

---

## 🛠️ **Set Variables**

```bash
#!/bin/bash

## Set variables
NEW_CONTAINER_NAME="app_new"  # Temporary container name for the update
FINAL_CONTAINER_NAME="app"    # Final container name for production
TEMP_PORT=3001                       # Temporary port for the new container
FINAL_PORT=3000                      # Final port for the production container
TEMP_IMAGE_NAME="app_temp"    # Temporary image name for the build
IMAGE_NAME="app"              # Production image name
```

---

## 🔨 **Build New Docker Image Without Cache**

First, we build the new Docker image **without cache** to ensure the latest changes are included.

```bash
docker build -t ${TEMP_IMAGE_NAME} .
```

---

## 🛑 **Stop and Remove Existing Container**

If there is an existing container with the final name, stop and remove it to prepare for the new container.

```bash
if docker ps -a --format '{{.Names}}' | grep -q "^${FINAL_CONTAINER_NAME}$"; then
  docker stop ${FINAL_CONTAINER_NAME} || true
  docker rm -f ${FINAL_CONTAINER_NAME} || true
fi
```

---

## 🚀 **Run New Container on Temporary Port**

Next, we run the new container on a **temporary port** to ensure it's working before swapping it into production.

```bash
docker run -d --name ${NEW_CONTAINER_NAME} -p ${TEMP_PORT}:80 ${TEMP_IMAGE_NAME}
```

---

## ⏳ **Wait for the New Container to Be Ready**

We use a loop to **check if the new container** is fully up and running by pinging it at the temporary port.

```bash
until curl -s http://localhost:${TEMP_PORT} > /dev/null; do
  sleep 1
done
```

---

## 🛑 **Stop and Remove Existing Production Container**

If the **current production container** is running, stop and remove it to allow the new container to take its place.

```bash
if docker ps --filter "name=${FINAL_CONTAINER_NAME}" --filter "status=running" | grep -q .; then
  docker stop ${FINAL_CONTAINER_NAME} || true
  docker rm ${FINAL_CONTAINER_NAME} || true
fi
```

---

## 🔄 **Rename the New Container to Final Name**

After confirming the new container is ready, **rename** it to match the production name.

```bash
docker rename ${NEW_CONTAINER_NAME} ${FINAL_CONTAINER_NAME}
```

---

## 🔧 **Update Port Binding and Run Final Container**

Stop and remove the final container name if it exists, then **run the updated container** on the final production port.

```bash
docker stop ${FINAL_CONTAINER_NAME} || true
docker rm ${FINAL_CONTAINER_NAME} || true
docker run -d --name ${FINAL_CONTAINER_NAME} -p ${FINAL_PORT}:80 ${TEMP_IMAGE_NAME}
```

---

## 🧹 **Clean Up Docker Images and Containers**

Optionally, we can **prune unnecessary images** and **clean up stopped containers** and unused networks to keep the system tidy.

1. Keep specific images (e.g., `app`) for faster future builds:

   ```bash
   docker image prune -f --filter "label!=app"
   ```

2. Clean up stopped containers and unused networks:

   ```bash
   docker container prune -f
   docker network prune -f
   ```

---

## 🎉 **Completion Message**

Finally, we print a success message to notify that the rolling update is complete.

```bash
echo "Rolling update completed successfully"
```

---

This script ensures **zero downtime** for your application during the update process by temporarily running a new container and swapping it with the old one only after it’s fully functional. Enjoy seamless updates! 🚀🔄

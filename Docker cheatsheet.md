# 🐳 Docker Cheatsheet

This cheatsheet is a quick reference to help you with the most commonly used Docker commands. Happy Docker-ing! 🚀

---

## 🚀 Getting Started

Before jumping into Docker commands, make sure you have Docker set up. You can follow the [official Docker guide](https://docs.docker.com/engine/installation/) for installation instructions. Once installed, use the following commands to check if Docker is working properly:

### 📝 *Post Installation Notes*:

If you need to prepend `sudo` to every command, follow the optional steps below.

* **Check Docker version**
  `docker version` - Displays full description of Docker version.

* **System Information**
  `docker info` - Displays system-wide Docker info.

* **Short Version Info**
  `docker -v` - Gives a brief version description.

* **Run a Test Container**
  `docker run hello-world` - Runs a test container.

### 🔧 Post Installation Steps (Optional)

To use Docker without `sudo`:

1. **Create the Docker group**:

   ```bash
   sudo groupadd docker
   ```

2. **Add your user to the Docker group**:

   ```bash
   sudo usermod -aG docker $USER
   ```

3. **Log out and back in** to re-evaluate group membership.

4. **Verify without sudo**:
   Run Docker commands without needing `sudo`.

---

## 📦 Docker Commands

### 🧐 **Get Docker Info**

| Command                                         | Description                            |
| ----------------------------------------------- | -------------------------------------- |
| `docker version`                                | Shows full version info.               |
| `docker -v`                                     | Short version description.             |
| `docker info`                                   | Displays system-wide info.             |
| `docker info --format '{{.DriverStatus}}'`      | Displays driver status.                |
| `docker info --format '{{json .DriverStatus}}'` | Displays driver status in JSON format. |

---

### 🖼️ **Manage Docker Images**

| Command                                             | Description                         |
| --------------------------------------------------- | ----------------------------------- |
| `docker image ls`                                   | List all local images.              |
| `docker image ls --filter 'reference=ubuntu:16.04'` | Filter images by name and tag.      |
| `docker image pull [image-name]`                    | Pull specified image from registry. |
| `docker image rm [image-name]`                      | Remove an image by name.            |
| `docker image prune`                                | Remove unused images.               |

---

### 🔍 **Search Docker Images**

| Command                                                  | Description                   |
| -------------------------------------------------------- | ----------------------------- |
| `docker search [image-name] --filter "is-official=true"` | Find official images.         |
| `docker search [image-name] --filter "stars=1000"`       | Find images with 1000+ stars. |

---

### 🛠️ **Manage Docker Containers**

#### Display Container Information

| Command                                                                               | Description                              |
| ------------------------------------------------------------------------------------- | ---------------------------------------- |
| `docker container ls`                                                                 | List all running containers.             |
| `docker container ls -a`                                                              | Show all containers regardless of state. |
| `docker container ls --filter "status=exited"`                                        | Show exited containers.                  |
| `docker container inspect [container-name]`                                           | Display detailed container info.         |
| `docker container inspect --format '{{.NetworkSettings.IPAddress}}' [container-name]` | Show IP address of the container.        |

#### Running Containers

| Command                                                     | Description                                                 |
| ----------------------------------------------------------- | ----------------------------------------------------------- |
| `docker container run [image-name]`                         | Run a container from an image.                              |
| `docker container run --rm [image-name]`                    | Run and automatically remove the container after execution. |
| `docker container run --name [container-name] [image-name]` | Assign a custom name to your container.                     |

#### Remove Containers

| Command                                                                  | Description                   |
| ------------------------------------------------------------------------ | ----------------------------- |
| `docker container rm [container-name]`                                   | Remove the container by name. |
| `docker container rm $(docker container ls --filter "status=exited" -q)` | Remove all exited containers. |

---

### 💾 **Manage Volumes**

#### Volume Information

| Command                                     | Description             |
| ------------------------------------------- | ----------------------- |
| `docker volume ls`                          | List all volumes.       |
| `docker volume ls --filter "dangling=true"` | List unused volumes.    |
| `docker volume inspect [volume-name]`       | Display volume details. |

#### Remove Volumes

| Command                                                            | Description                |
| ------------------------------------------------------------------ | -------------------------- |
| `docker volume rm [volume-name]`                                   | Remove a specified volume. |
| `docker volume rm $(docker volume ls --filter "dangling=true" -q)` | Remove unused volumes.     |

---

## 🏃‍♂️ Running Containers

### 🐋 Run a Hello-World Container

```bash
docker run hello-world
```

### 🔥 Run an Alpine Linux Container (Lightweight!)

1. Find and display the Alpine image:

   ```bash
   docker search alpine --filter=stars=1000 --no-trunc
   ```

2. Pull the Alpine image:

   ```bash
   docker pull alpine
   ```

3. List your local images:

   ```bash
   docker image ls
   ```

4. List contents of the Alpine container:

   ```bash
   docker run alpine ls -l
   ```

5. Print a message from Alpine:

   ```bash
   docker run alpine echo "Hello from Alpine!"
   ```

6. Run Alpine interactively with a shell:

   ```bash
   docker run -it alpine bin/sh
   ```

---

### 🐱 Run MongoDB

#### MongoDB with Named Volume

```bash
docker run --rm --name mongo-dev -v mongo-dev-db:/data/db -d mongo
```

#### MongoDB with Bind Mount

```bash
cd
mkdir -p mongodb/data/db
docker run --rm --name mongo-dev -v ~/mongodb/data/db:/data/db -d mongo
```

#### Access MongoDB

1. **Connect to MongoDB container**:

   ```bash
   docker exec -it mongo-dev bash
   ```

2. **Open MongoDB shell**:

   ```bash
   mongo localhost
   ```

3. **Show Databases**:

   ```bash
   show dbs
   ```

4. **Create a New Database**:

   ```javascript
   use test
   db.messages.insert({"message": "Hello World!"})
   db.messages.find()
   ```

---

## 📸 Creating Custom Images

### 🐏 Alpine Linux with Git

1. Create project folder and Dockerfile:

   ```bash
   mkdir -p projects/docker/alpine-git
   cd projects/docker/alpine-git
   touch Dockerfile
   ```

2. Create your `Dockerfile`:

   ```docker
   FROM alpine:latest
   LABEL author="your-name" description="Alpine with Git & Vim"
   RUN apk update && apk add git vim
   WORKDIR /projects
   ```

3. Build and run the image:

   ```bash
   docker image build -t [your-username]/alpine-git .
   docker run --rm -it [your-username]/alpine-git /bin/sh
   ```

4. Push to Docker Hub:

   ```bash
   docker login
   docker push [your-username]/alpine-git:latest
   ```

---

🎓 **Additional Resources**
For more information and tutorials, check out:

* [Docker Documentation](https://docs.docker.com/engine/installation/)
* [Free Docker Training](https://training.docker.com)
* [Docker Hub Repository](https://hubs.docker.com)

---

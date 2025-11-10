## 🐳 **Docker CLI Cheat Sheet**

| **Category**                   | **Command**                                                       | **Description / Notes**                                               |
| ------------------------------ | ----------------------------------------------------------------- | --------------------------------------------------------------------- |
| 🔍 **Info & Version**          | `docker info`                                                     | Display system-wide Docker information                                |
|                                | `docker version`                                                  | Display Docker client and server version                              |
| 🔐 **Registry & Images**       | `docker login`                                                    | Log in to a Docker registry (e.g., Docker Hub)                        |
|                                | `docker pull [imageName]`                                         | Pull an image from a registry                                         |
|                                | `docker image inspect [imageName]`                                | Show detailed image information                                       |
| ▶️ **Run & Create Containers** | `docker run [imageName]`                                          | Run a container from an image                                         |
|                                | `docker run -d [imageName]`                                       | Run container in **detached** mode (background)                       |
|                                | `docker run -it [imageName] /bin/bash`                            | Run container **interactively** with Bash shell                       |
|                                | `docker run -it mcr.microsoft.com/powershell:nanoserver pwsh.exe` | Run a Windows NanoServer container with PowerShell                    |
|                                | `docker run --name [containerName] [imageName]`                   | Assign a custom name to the container                                 |
|                                | `docker run --publish 8080:80 --name webserver nginx`              | Run Nginx container and map port 8080 of host to 80 of container      |
| ⚙️ **Resource Limits**         | `docker run --memory="256m" nginx`                                | Limit container memory to 256 MB                                      |
|                                | `docker run --cpus=".5" nginx`                                    | Limit container CPU usage to 0.5 cores                                |
| 🚀 **Container Lifecycle**     | `docker start [containerName]`                                    | Start a stopped container                                             |
|                                | `docker stop [containerName]`                                     | Stop a running container gracefully                                   |
|                                | `docker kill [containerName]`                                     | Force-stop (kill) a running container                                 |
|                                | `docker rm [containerName]`                                       | Remove a stopped container                                            |
| 📜 **List Containers**         | `docker ps`                                                       | List **running** containers                                           |
|                                | `docker ps -a`                                                    | List **all** containers (running + stopped)                           |
| 🔗 **Attach / Exec Shell**     | `docker run -it nginx /bin/bash`                                  | Start a new Nginx container with Bash shell                           |
|                                | `docker run -it mcr.microsoft.com/powershell:nanoserver pwsh.exe` | Start new PowerShell session in NanoServer container                  |
|                                | `docker exec -it [containerName] /bin/bash`                       | Attach Bash shell to a **running** Linux container                    |
|                                | `docker exec -it [containerName] pwsh.exe`                        | Attach PowerShell shell to a **running** Windows container            |
|                                | `docker attach [containerName]`                                   | Attach to the container’s main process (less preferred for debugging) |
| 🔄 **Detach & Exit Safely**    | `Ctrl + P, Ctrl + Q`                                              | Detach from container without stopping it                             |
| 🧹 **Cleanup**                 | `docker rm $(docker ps -aq)`                                      | Remove all stopped containers                                         |
|                                | `docker rmi [imageName]`                                          | Remove an image                                                       |
|                                | `docker system prune`                                             | Remove unused containers, images, and networks                        |

---

### 🧽 **Docker Cleanup Commands (Detailed)**

| **Command**                    | **Description**                                 |
| ------------------------------ | ----------------------------------------------- |
| `docker rm [containerName]`    | Removes stopped containers                      |
| `docker rm $(docker ps -a -q)` | Removes all stopped containers                  |
| `docker images`                | Lists images                                    |
| `docker rmi [imageName]`       | **Deletes the image**                           |
| `docker system prune -a`       | Removes all images not in use by any containers |


## 🐳 **Docker CLI Cheat Sheet**

| **Category**                   | **Command**                                                       | **Description / Notes**                                               |
|  | -- |  |
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



### 🧽 **Docker Cleanup Commands (Detailed)**

| **Command**                    | **Description**                                 |
|  | -- |
| `docker rm [containerName]`    | Removes stopped containers                      |
| `docker rm $(docker ps -a -q)` | Removes all stopped containers                  |
| `docker images`                | Lists images                                    |
| `docker rmi [imageName]`       | **Deletes the image**                           |
| `docker system prune -a`       | Removes all images not in use by any containers |



### 🏗️ 1. Running Containers Interactively

* Command:

  ```bash
  docker run -it nginx /bin/bash
  ```
* Since `nginx:latest` wasn’t found locally, Docker **pulled the image from Docker Hub** automatically.
* Once inside, you’re in the **container’s root shell** — verified by `root@<container_id>:/#`.
* You can inspect the file system using `ls`, etc.
* To exit:

  ```bash
  exit
  ```

### 🌐 2. Running Nginx in Detached Mode with Port Mapping

* Command:

  ```bash
  docker run -p 8080:80 --name webserver nginx
  ```
* Meaning:

  * `-p 8080:80` maps **host port 8080 → container port 80**.
  * `--name webserver` gives the container a readable name.
* Logs confirm Nginx started successfully and served traffic.
* Access via:
  👉 [http://localhost:8080](http://localhost:8080)
* The favicon warning (`404 favicon.ico`) is normal — Nginx image doesn’t include one.


### ⚠️ 3. Common Syntax Mistakes (and What They Teach You)

| Mistake                                                        | What Went Wrong                                    | Lesson                                                           |
| -- | -- | - |
| `docker run -p -d 8080:80`                                     | Wrong flag order; `-p` and `-d` are separate flags | Always place flags before values (e.g., `-d -p 8080:80`)         |
| `docker run -p -8080:80`                                       | Negative port number parsed (`-8080`)              | No `-` before port; use `8080:80`                                |
| `docker run -detach -publish -8080:80`                         | Misused long flags and negative number             | Use `--detach --publish 8080:80`                                 |
| `docker run --detach --publish 8080:80 --name webserver nginx` | Correct ✅                                          | Demonstrates how multiple flags work properly together           |
| `docker run ... --name webserver nginx` (2nd time)             | Error: “name already in use”                       | Each container name must be **unique**; remove or rename old one |
| Fix: `docker rm webserver`                                     | Remove old container before recreating it          |                                                                  |


### 🧩 4. Executing Commands Inside Running Containers

* Correct command:

  ```bash
  docker exec -it webserver /bin/bash
  ```

  ✅ Opens a **bash shell** inside a running container.
* syntax:

    ```
    docker exec -it <container_name> <command>
    ```



### 🪟 5. Running Windows Containers (and Why It Failed)

* Tried:

  ```bash
  docker run -it mcr.microsoft.com/windows/nanoserver:ltsc2025 pwsh.exe
  ```
* Error:

  ```
  no matching manifest for linux/amd64 in the manifest list entries
  ```
* Meaning:

  * You’re running **Docker Desktop with Linux containers**, but `nanoserver` is a **Windows-only image**.
  * To fix: Switch Docker to **Windows container mode** (from Docker Desktop menu).
* Lesson:
  🧩 *Linux vs Windows containers are mutually exclusive modes.*


### 🧹 6. Container Lifecycle Recap

| Action                   | Command                  | Notes                               |
|  |  | -- |
| List all containers      | `docker ps -a`           | Shows running and stopped           |
| Start existing container | `docker start webserver` | Reuse a stopped container           |
| Stop running container   | `docker stop webserver`  | Graceful stop                       |
| Remove container         | `docker rm webserver`    | Delete container (must be stopped)  |
| List images              | `docker images`          | See pulled images                   |
| Remove image             | `docker rmi nginx`       | Delete local image                  |
| Cleanup unused           | `docker system prune -a` | Remove all unused images/containers |



### 🧭 7. Key Concepts Reinforced

* Containers are **ephemeral** — exiting the shell doesn’t delete them.
* Names (`--name`) make containers easy to reference.
* `-it` flag = interactive + TTY.
* `-d` flag = detached mode.
* You can **attach anytime** to inspect or debug a running container.
* Docker automatically handles:

  * Image download (if not found locally)
  * Logging to console
  * Graceful shutdown on `Ctrl+C` (SIGINT)


## 🧩 `-it` in Docker (short for `--interactive --tty`)

When you see `-it` used in a Docker command like:

```bash
docker run -it nginx /bin/bash
```

…it’s actually a combination of **two separate flags**:

| Flag | Full Form       | Meaning                                                   | Example Behavior                                                                       |
| ---- | --------------- | --------------------------------------------------------- | -------------------------------------------------------------------------------------- |
| `-i` | `--interactive` | Keeps STDIN (standard input) open — even if not attached. | Allows you to **type commands** inside the container.                                  |
| `-t` | `--tty`         | Allocates a **pseudo-TTY (terminal)**.                    | Gives you a proper **shell-like interface** with prompt, colors, cursor movement, etc. |

So together:

> `-it` = **interactive terminal session**

It’s what makes the container behave like a normal shell session instead of just running in the background.


### 🧠 Example Breakdown

**Without `-it`:**

```bash
docker run ubuntu
```

The container starts and immediately exits — because it has nothing to run interactively (no terminal input is attached).


**With `-it`:**

```bash
docker run -it ubuntu /bin/bash
```

You get:

```
root@abcd1234:/#
```

Now you can type commands inside the container — it behaves like an isolated Linux shell.


### 🧱 Why `-it` Is Commonly Used

* For **debugging** or exploring containers.
* To test **configurations or files** inside containers.
* To **inspect running environments** interactively (e.g., check `/etc/nginx/nginx.conf`).


### ⚙️ Common Pairs with `-it`

| Command                               | Purpose                                    |
| ------------------------------------- | ------------------------------------------ |
| `docker run -it ubuntu /bin/bash`     | Start new Ubuntu container interactively   |
| `docker exec -it webserver /bin/bash` | Attach Bash shell to a *running* container |
| `docker run -it alpine sh`            | Use Alpine’s lightweight shell             |


### In Summary:

> `-it` = `--interactive --tty`
> → Keeps STDIN open and gives you a real terminal experience inside the container.

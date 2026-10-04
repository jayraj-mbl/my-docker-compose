# 🐳 Docker Compose – Multiple Nginx Web Servers

A simple Docker Compose project that runs **two independent Nginx web servers** using Docker containers. Each container serves a separate HTML page and is accessible through a different port on the host machine.

This project demonstrates how to use **Docker Compose, containerization, port mapping, and volume mounting** to deploy multiple web services simultaneously.

## 📌 Project Overview

This project uses Docker Compose to deploy two Nginx web servers:

* **Web One:** Accessible at `http://localhost:8081`
* **Web Two:** Accessible at `http://localhost:8082`

Both web servers use the lightweight `nginx:alpine` Docker image and serve custom HTML pages from the local system using bind mounts.

## 🏗️ Architecture

```text
                 Host Machine
                      |
             Docker Compose
                      |
          +-----------+-----------+
          |                       |
     Web One                  Web Two
          |                       |
   Nginx Container          Nginx Container
          |                       |
     Port 8081                 Port 8082
          |                       |
     Container 80             Container 80
          |                       |
     web1/index.html          web2/index.html
          |                       |
          +-----------+-----------+
                      |
                Web Browser
                      |
       +--------------+--------------+
       |                             |
 http://localhost:8081       http://localhost:8082
```

## 📂 Project Structure

```text
docker-compose-nginx/
│
├── docker-compose.yml
│
├── web1/
│   └── index.html
│
├── web2/
│   └── index.html
│
└── README.md
```

## 🛠️ Technologies Used

* **Docker:** Containerization platform
* **Docker Compose:** Multi-container application management
* **Nginx:** Lightweight web server
* **Alpine Linux:** Lightweight Linux-based image
* **HTML:** Web page content

## ⚙️ Docker Compose Configuration

The following `docker-compose.yml` file defines two Nginx services.

```yaml
version: '3.8'

services:
  web_one:
    image: nginx:alpine
    container_name: nginx_container_one
    ports:
      - "8081:80"
    volumes:
      - ./web1/index.html:/usr/share/nginx/html/index.html:ro

  web_two:
    image: nginx:alpine
    container_name: nginx_container_two
    ports:
      - "8082:80"
    volumes:
      - ./web2/index.html:/usr/share/nginx/html/index.html:ro
```

### 🔍 Configuration Explained

| Configuration         | Description                                 |
| --------------------- | ------------------------------------------- |
| `version: '3.8'`      | Specifies the Compose file format           |
| `services`            | Defines the containers to run               |
| `web_one`             | First Nginx service                         |
| `web_two`             | Second Nginx service                        |
| `image: nginx:alpine` | Uses the lightweight Nginx image            |
| `container_name`      | Assigns a custom container name             |
| `8081:80`             | Maps host port 8081 to container port 80    |
| `8082:80`             | Maps host port 8082 to container port 80    |
| `volumes`             | Mounts local HTML files into the containers |
| `:ro`                 | Mounts the files as read-only               |

## 🚀 Getting Started

### 1. Prerequisites

Make sure you have installed:

* [Docker Desktop](https://www.docker.com/products/docker-desktop/) (Windows/macOS) or Docker Engine (Linux)
* Docker Compose (included in current Docker Desktop versions)
* Git

### 2. Clone the Repository

```bash
git clone https://github.com/YOUR_USERNAME/docker-compose-nginx.git
```

Navigate to the project directory:

```bash
cd docker-compose-nginx
```

### 3. Create HTML Pages

Create the `web1` and `web2` directories, each containing an `index.html` file.

**web1/index.html**

```html
<!DOCTYPE html>
<html>
<head>
    <title>Web Server One</title>
</head>
<body>
    <h1>Welcome to Web Server One</h1>
    <p>This page is served by Nginx Container One.</p>
</body>
</html>
```

**web2/index.html**

```html
<!DOCTYPE html>
<html>
<head>
    <title>Web Server Two</title>
</head>
<body>
    <h1>Welcome to Web Server Two</h1>
    <p>This page is served by Nginx Container Two.</p>
</body>
</html>
```

### 4. Start the Containers

Run the following command from the project directory:

```bash
docker compose up -d
```

This command will:

* Download the `nginx:alpine` image if it is not already available.
* Create two Nginx containers.
* Map the host ports to their respective container ports.
* Mount the local HTML files.
* Start both web servers in detached mode.

### 5. Verify the Containers

Check the running containers:

```bash
docker ps
```

You should see both `nginx_container_one` and `nginx_container_two` running.

### 6. Access the Web Servers

Open your browser and visit:

| Web Server          | URL                   |
| ------------------- | --------------------- |
| Nginx Container One | http://localhost:8081 |
| Nginx Container Two | http://localhost:8082 |

Each URL should display its respective HTML page.

## 🧰 Useful Docker Commands

| Command                                  | Purpose                                 |
| ---------------------------------------- | --------------------------------------- |
| `docker compose up -d`                   | Start all services in the background    |
| `docker compose down`                    | Stop and remove containers and network  |
| `docker compose ps`                      | Display service status                  |
| `docker compose logs`                    | View logs from all services             |
| `docker compose logs web_one`            | View logs from Web One                  |
| `docker compose logs web_two`            | View logs from Web Two                  |
| `docker restart nginx_container_one`     | Restart the first container             |
| `docker restart nginx_container_two`     | Restart the second container            |
| `docker exec -it nginx_container_one sh` | Open a shell inside the first container |
| `docker images`                          | List locally available images           |

## 🔄 Updating HTML Pages

Since the HTML files are bind-mounted, you can edit them directly on your host machine.

For example, update:

```text
web1/index.html
```

Refresh `http://localhost:8081` in your browser to see the changes. Nginx serves the updated file without needing to rebuild the image or recreate the container.

## 🛑 Stopping the Project

To stop and remove the containers and the Compose-created network:

```bash
docker compose down
```

Your local HTML files remain unchanged.

## 🐞 Troubleshooting

**1. Port already in use**

If port `8081` or `8082` is already occupied, change the host-side port in the Compose file.

For example:

```yaml
ports:
  - "8083:80"
```

**2. Container is not starting**

Check the container logs:

```bash
docker compose logs
```

**3. Web page is not loading**

Verify that both containers are running:

```bash
docker compose ps
```

Check whether the ports are mapped correctly and ensure that the HTML files exist at the expected paths.

**4. HTML file changes are not visible**

Refresh the browser. If needed, perform a hard refresh (`Ctrl + F5`) to bypass the browser cache.

## 🎯 Key Learnings

By completing this project, you will gain hands-on experience with:

* Creating and managing multiple Docker containers.
* Writing Docker Compose YAML configuration files.
* Understanding host-to-container port mapping.
* Using bind mounts to serve static web content.
* Running containers in detached mode.
* Monitoring containers and analyzing logs.
* Troubleshooting basic Docker networking and web server issues.

## 🔮 Future Enhancements

* Add an Nginx reverse proxy to route traffic to multiple services.
* Configure a custom Docker network.
* Enable HTTPS using SSL/TLS certificates.
* Add health checks to monitor container availability.
* Deploy the project on a cloud-based Linux server.
* Integrate the project into a CI/CD pipeline using GitHub Actions.

## 👨‍💻 Author

**Jayraj M**

Application Support Engineer | Docker | Linux | SQL | DevOps

GitHub: [YOUR_GITHUB_PROFILE](https://github.com/YOUR_USERNAME)

---

⭐ If you find this project useful, feel free to star the repository!

# 📝 Docker-Based Full Stack Todo App with Nginx Reverse Proxy, HTTPS, and CI/CD

A full stack Todo app built using **MongoDB, Express.js, and React**, containerized with **Docker**, orchestrated via **Docker Compose**, and deployed on **AWS EC2** with a **root-level Nginx reverse proxy** and **HTTPS support (Let's Encrypt)**.

---

## 🛠️ Technologies Used

- **Frontend**: React
- **Backend**: Express.js (Node.js)
- **Database**: MongoDB
- **Containerization**: Docker & Docker Compose
- **Web Server**: Nginx
- **SSL**: Let's Encrypt + Certbot
- **CI/CD**: GitHub Actions (optional)
- **Cloud Deployment**: AWS EC2

---

## 🚀 Project Structure

```
docker-todo-app/
├── backend/
│   └── Dockerfile
├── frontend/
│   └── Dockerfile
├── nginx/
│   └── nginx.conf
├── docker-compose.yml
└── README.md
```

---

## ⚙️ Step-by-Step Guide

### ✅ Step 1: Clone the Repository

```bash
git clone https://github.com/YOUR_USERNAME/docker-todo-app.git
cd docker-todo-app
```

---

### ✅ Step 2: Build and Tag Docker Images

```bash
cd backend
docker build -t todo-backend .
cd ../frontend
docker build -t todo-frontend .
```

---

### ✅ Step 3: Push Images to Docker Hub

```bash
docker tag todo-backend dockeruserari/todo-backend
docker tag todo-frontend dockeruserari/todo-frontend

docker push dockeruserari/todo-backend
docker push dockeruserari/todo-frontend
```

---

### ✅ Step 4: Set Up AWS EC2
 Launch Ubuntu EC2 instance.

Install Docker & Docker Compose.

 Pull your project and run

```bash
# Launch Ubuntu EC2, SSH in:
ssh -i your-key.pem ubuntu@your-ec2-ip

# Install Docker & Compose
sudo apt update
sudo apt install docker.io docker-compose -y
sudo usermod -aG docker $USER
```

Clone and deploy:
```bash
git clone https://github.com/YOUR_USERNAME/docker-todo-app.git
cd docker-todo-app
docker-compose up -d
```

---

### ✅ Step 5: Setup Nginx Reverse Proxy

Create `/etc/nginx/sites-available/todoapp.conf`:

```nginx
server {
    listen 80;
    server_name arihantdugge.com;

    location / {
        proxy_pass http://localhost:3000;
    }

    location /api/ {
        proxy_pass http://localhost:5000/;
    }
}
```

Enable config:
```bash
sudo ln -s /etc/nginx/sites-available/todoapp.conf /etc/nginx/sites-enabled/
sudo systemctl restart nginx
```

---

### ✅ Step 6: Configure Domain DNS

- Log in to Hostinger
- Go to DNS management
- Add an A record: `arihantdugge.com` → your EC2 public IP

---

### ✅ Step 7: Add HTTPS with Let’s Encrypt

```bash
sudo apt install certbot python3-certbot-nginx -y
sudo certbot --nginx -d arihantdugge.com
```

---

### ✅ Step 8: GitHub Actions CI/CD (Optional)

Create `.github/workflows/docker.yml` with build & push steps.

---

## ✅ Live Demo

🌍 [https://arihantdugge.com](https://arihantdugge.com)

---

## 📦 docker-compose.yml

```yaml
version: "3"

services:
  mongo:
    image: mongo
    container_name: mongodb
    ports:
      - "27017:27017"
    volumes:
      - mongo_data:/data/db

  backend:
    image: dockeruserari/todo-backend
    container_name: todo-backend
    ports:
      - "5000:5000"
    environment:
      - MONGO_URL=mongodb://mongodb:27017/todos
    depends_on:
      - mongo

  frontend:
    image: dockeruserari/todo-frontend
    container_name: todo-frontend
    depends_on:
      - backend

volumes:
  mongo_data:
```

---

## 🙌 Author

**Arihant Dugge**  
📫 [https://arihantdugge.com](https://arihantdugge.com)  
🐳 DockerHub: [dockeruserari](https://hub.docker.com/u/dockeruserari)

---

## 📜 License

MIT License

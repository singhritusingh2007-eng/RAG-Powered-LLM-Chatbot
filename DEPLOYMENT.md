# 🚀 Deployment Guide for RAG-Powered LLM Chatbot

Complete step-by-step guides for deploying the chatbot to production environments.

## Table of Contents

1. [Streamlit Cloud (Recommended)](#streamlit-cloud-recommended)
2. [Docker Deployment](#docker-deployment)
3. [AWS EC2](#aws-ec2)
4. [Heroku](#heroku)
5. [Local Deployment](#local-deployment)

---

## Streamlit Cloud (Recommended)

### Pros
- ✅ Free tier available
- ✅ Automatic SSL/HTTPS
- ✅ Easy GitHub integration
- ✅ One-click deployment
- ✅ Built-in monitoring

### Prerequisites
- GitHub account with your repository
- Hugging Face API token

### Steps

1. **Commit and Push to GitHub**
   ```bash
   cd RAG-Powered-LLM-Chatbot
   git add .
   git commit -m "Production ready RAG chatbot"
   git push origin main
   ```

2. **Go to Streamlit Cloud**
   - Visit [share.streamlit.io](https://share.streamlit.io)
   - Click **"New app"**

3. **Connect Repository**
   - Select your GitHub account
   - Select repository: `RAG-Powered-LLM-Chatbot`
   - Select branch: `main`
   - Select main file path: `chatbot_streamlit_combined.py`

4. **Add Secrets**
   - Click **"Advanced settings"** → **"Secrets"**
   - Add the following:
   ```toml
   HUGGINGFACEHUB_API_TOKEN = "hf_your_token_here"
   ```

5. **Deploy**
   - Click **"Deploy"**
   - Wait for deployment to complete (2-5 minutes)
   - Your app will be live at: `https://app-name.streamlit.app`

### Troubleshooting

**Issue**: "ModuleNotFoundError: No module named 'X'"
- **Solution**: Ensure all dependencies are in `requirements.txt`
- Run: `pip freeze > requirements.txt`

**Issue**: "API Token not working"
- **Solution**: Check secrets are correctly added
- Go to App Settings → Secrets → Verify token

**Issue**: "Vector store not found"
- **Solution**: Push `vector store/` directory to GitHub
- Create `.gitkeep` file in `vector store/` to preserve directory

---

## Docker Deployment

### Prerequisites
- Docker installed locally or on server
- Docker Hub account (for hosting images)

### Dockerfile

```dockerfile
# Use official Python runtime
FROM python:3.10-slim

# Set working directory
WORKDIR /app

# Copy requirements
COPY requirements.txt .

# Install dependencies
RUN pip install --no-cache-dir -r requirements.txt

# Copy application
COPY . .

# Expose port
EXPOSE 8501

# Set streamlit config
RUN mkdir -p ~/.streamlit && \
    echo "[server]" > ~/.streamlit/config.toml && \
    echo "headless = true" >> ~/.streamlit/config.toml && \
    echo "enableXsrfProtection = false" >> ~/.streamlit/config.toml

# Run app
CMD ["streamlit", "run", "chatbot_streamlit_combined.py"]
```

### Build and Run Locally

1. **Build Docker Image**
   ```bash
   docker build -t rag-chatbot:latest .
   ```

2. **Run Container**
   ```bash
   docker run -p 8501:8501 \
     -e HUGGINGFACEHUB_API_TOKEN="your_token_here" \
     rag-chatbot:latest
   ```

3. **Access App**
   - Open: `http://localhost:8501`

### Push to Docker Hub

1. **Login to Docker Hub**
   ```bash
   docker login
   ```

2. **Tag Image**
   ```bash
   docker tag rag-chatbot:latest username/rag-chatbot:latest
   ```

3. **Push Image**
   ```bash
   docker push username/rag-chatbot:latest
   ```

### Docker Compose (Multi-service)

```yaml
version: '3.8'

services:
  chatbot:
    build: .
    ports:
      - "8501:8501"
    environment:
      - HUGGINGFACEHUB_API_TOKEN=${HUGGINGFACEHUB_API_TOKEN}
    volumes:
      - ./vector store:/app/vector store
    restart: unless-stopped

  nginx:
    image: nginx:alpine
    ports:
      - "80:80"
      - "443:443"
    volumes:
      - ./nginx.conf:/etc/nginx/nginx.conf:ro
      - ./ssl:/etc/nginx/ssl:ro
    depends_on:
      - chatbot
    restart: unless-stopped
```

**Run with Docker Compose**
```bash
docker-compose up -d
```

---

## AWS EC2

### Prerequisites
- AWS account
- EC2 instance (t3.medium or larger)
- Ubuntu 22.04 LTS
- Elastic IP (optional but recommended)

### Steps

1. **Launch EC2 Instance**
   - Instance type: `t3.medium` (minimum for inference)
   - Storage: `30GB` (GP3)
   - Security group: Allow ports 22 (SSH), 80 (HTTP), 443 (HTTPS)

2. **Connect via SSH**
   ```bash
   ssh -i your-key.pem ubuntu@your-instance-ip
   ```

3. **Install Dependencies**
   ```bash
   sudo apt update && sudo apt upgrade -y
   sudo apt install -y python3-pip python3-venv git curl wget
   sudo apt install -y nvidia-driver-535 nvidia-utils  # GPU support
   ```

4. **Clone Repository**
   ```bash
   git clone https://github.com/singhritusingh2007-eng/RAG-Powered-LLM-Chatbot.git
   cd RAG-Powered-LLM-Chatbot
   ```

5. **Create Virtual Environment**
   ```bash
   python3 -m venv venv
   source venv/bin/activate
   ```

6. **Install Requirements**
   ```bash
   pip install -r requirements.txt
   ```

7. **Set Environment Variables**
   ```bash
   echo 'HUGGINGFACEHUB_API_TOKEN="your_token"' > .env
   ```

8. **Create Systemd Service**
   ```bash
   sudo tee /etc/systemd/system/rag-chatbot.service > /dev/null <<EOF
   [Unit]
   Description=RAG Chatbot Streamlit App
   After=network.target

   [Service]
   Type=simple
   User=ubuntu
   WorkingDirectory=/home/ubuntu/RAG-Powered-LLM-Chatbot
   Environment="PATH=/home/ubuntu/RAG-Powered-LLM-Chatbot/venv/bin"
   EnvironmentFile=/home/ubuntu/RAG-Powered-LLM-Chatbot/.env
   ExecStart=/home/ubuntu/RAG-Powered-LLM-Chatbot/venv/bin/streamlit run chatbot_streamlit_combined.py --server.port=8501 --server.address=0.0.0.0
   Restart=always
   RestartSec=10

   [Install]
   WantedBy=multi-user.target
   EOF
   ```

9. **Start Service**
   ```bash
   sudo systemctl daemon-reload
   sudo systemctl enable rag-chatbot
   sudo systemctl start rag-chatbot
   ```

10. **Set Up Nginx Reverse Proxy**
    ```bash
    sudo apt install -y nginx
    sudo tee /etc/nginx/sites-available/default > /dev/null <<EOF
    server {
        listen 80 default_server;
        listen [::]:80 default_server;

        server_name _;

        location / {
            proxy_pass http://localhost:8501;
            proxy_http_version 1.1;
            proxy_set_header Upgrade \$http_upgrade;
            proxy_set_header Connection "upgrade";
            proxy_set_header Host \$host;
            proxy_set_header X-Real-IP \$remote_addr;
        }
    }
    EOF
    ```

11. **Start Nginx**
    ```bash
    sudo systemctl restart nginx
    ```

12. **Access Application**
    - Open: `http://your-instance-ip`

---

## Heroku

### Prerequisites
- Heroku account
- Heroku CLI installed
- Git

### Steps

1. **Create Procfile**
   ```bash
   echo "web: streamlit run chatbot_streamlit_combined.py --server.port \$PORT" > Procfile
   ```

2. **Create Heroku App**
   ```bash
   heroku create your-app-name
   ```

3. **Set Environment Variables**
   ```bash
   heroku config:set HUGGINGFACEHUB_API_TOKEN="your_token"
   ```

4. **Push to Heroku**
   ```bash
   git push heroku main
   ```

5. **View Logs**
   ```bash
   heroku logs --tail
   ```

**Note**: Heroku free tier has been discontinued. Use paid dynos or try alternatives.

---

## Local Deployment

### Setup

1. **Clone and Install**
   ```bash
   git clone <your-repo>
   cd RAG-Powered-LLM-Chatbot
   pip install -r requirements.txt
   ```

2. **Configure Environment**
   ```bash
   cp .env.example .env
   # Edit .env with your token
   ```

3. **Run Application**
   ```bash
   streamlit run chatbot_streamlit_combined.py
   ```

4. **Access**
   - Open: `http://localhost:8501`

### Production Setup with Supervisor

1. **Install Supervisor**
   ```bash
   pip install supervisor
   ```

2. **Create Configuration**
   ```bash
   sudo tee /etc/supervisor/conf.d/rag-chatbot.conf > /dev/null <<EOF
   [program:rag-chatbot]
   command=/path/to/venv/bin/streamlit run /path/to/chatbot_streamlit_combined.py
   directory=/path/to/RAG-Powered-LLM-Chatbot
   autostart=true
   autorestart=true
   redirect_stderr=true
   stdout_logfile=/var/log/rag-chatbot.log
   EOF
   ```

3. **Start Service**
   ```bash
   sudo supervisorctl reread
   sudo supervisorctl update
   sudo supervisorctl start rag-chatbot
   ```

---

## Performance Optimization

### Memory Management
```python
import torch
import gc

# Clear GPU memory
torch.cuda.empty_cache()
gc.collect()
```

### Caching
```python
@st.cache_resource
def load_model():
    # Load once and reuse
    return prepare_rag_llm(...)
```

### Scaling
- Use load balancer (Nginx) for multiple instances
- Implement Redis for session management
- Use CDN for static assets

---

## Monitoring and Logging

### Streamlit Metrics
```bash
streamlit logger --level debug
```

### Application Health Check
```bash
curl http://localhost:8501/healthz
```

### Log Rotation
```bash
sudo apt install logrotate
```

---

## Security Best Practices

- ✅ Use environment variables for secrets
- ✅ Enable HTTPS with SSL certificates
- ✅ Implement rate limiting
- ✅ Add authentication if needed
- ✅ Keep dependencies updated
- ✅ Use firewall rules
- ✅ Regular backups of vector stores

---

## Troubleshooting

### Issue: High Memory Usage
**Solution**: Reduce batch size or chunk size
```python
chunk_size = 100  # Reduce from 200
```

### Issue: Slow Response Time
**Solution**: Enable GPU acceleration or use smaller model

### Issue: Vector Store Upload Fails
**Solution**: Ensure disk space available
```bash
df -h
```

---

## Cost Estimation

| Platform | Cost/Month | Notes |
|----------|-----------|-------|
| Streamlit Cloud | Free-$100 | Recommended |
| AWS EC2 t3.medium | $30-50 | On-demand |
| Docker Hub | Free | 1 free private repo |
| Heroku Dyno | $50-500 | Paid only |

---

**For support and questions, open an issue on GitHub!**
# Data Loader Clay - Complete Setup Guide for Linux

A Docker-based data processing tool for working with maps, satellite images, and machine learning through Jupyter Notebooks.

## 🚀 Quick Start

1. **Install Docker** (see Installation section below)
2. **Clone this repository**
3. **Run the container**
4. **Open Jupyter Notebook in your browser**
5. **Download processed images**

## 📋 Prerequisites

- Linux operating system (Ubuntu 18.04+, CentOS 7+, etc.)
- Internet connection
- Basic terminal knowledge

## 🐳 Docker Installation for Linux

### Step 1: Install Docker

```bash
# Update package list
sudo apt-get update

# Install required packages
sudo apt-get install -y \
    apt-transport-https \
    ca-certificates \
    curl \
    gnupg \
    lsb-release

# Add Docker's official GPG key
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg

# Add Docker repository
echo \
  "deb [arch=amd64 signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu \
  $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

# Update package list again
sudo apt-get update

# Install Docker Engine
sudo apt-get install -y docker-ce docker-ce-cli containerd.io

# Install Docker Compose
sudo curl -L "https://github.com/docker/compose/releases/download/v2.20.0/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
sudo chmod +x /usr/local/bin/docker-compose
```

### Step 2: Add User to Docker Group

```bash
# Add your user to the docker group
sudo usermod -aG docker $USER

# Apply the new group membership
newgrp docker

# Verify Docker works without sudo
docker --version
```

### Step 3: Start Docker Service

```bash
# Start Docker service
sudo systemctl start docker

# Enable Docker to start on boot
sudo systemctl enable docker

# Verify Docker is running
sudo systemctl status docker
```

## 📥 Download and Setup

### Step 1: Clone the Repository

```bash
# Navigate to your desired directory
cd ~/Desktop

# Clone the repository
git clone https://github.com/YOUR_USERNAME/DATA-LOADER_CLAY.git

# Navigate into the project directory
cd DATA-LOADER_CLAY
```

### Step 2: Verify Files

```bash
# List all files to ensure everything is present
ls -la

# You should see:
# - Dockerfile
# - docker-compose.yml
# - requirements.txt
# - data_process.ipynb
# - README.md
```

## 🏗️ Build and Run

### Step 1: Build the Docker Image

```bash
# Build the Docker image (first time only)
docker-compose build --no-cache

# This may take 5-10 minutes on first run
# Subsequent builds will be faster
```

### Step 2: Start the Container

```bash
# Start the container in detached mode
docker-compose up -d

# Verify the container is running
docker ps

# You should see a container named "data-loader-clay" running on port 8888
```

### Step 3: Access Jupyter Notebook

1. **Open your web browser**
2. **Navigate to:** `http://localhost:8888`
3. **You should see the Jupyter file browser**
4. **Click on `data_process.ipynb` to open the notebook**

## 📊 Using the Notebook

### Step 1: Open the Notebook

- In Jupyter, click on `data_process.ipynb`
- The notebook will open with multiple cells

### Step 2: Download Processed Images

After running all cells, you'll see a message like:
```
"Saved X images to 'processed_images/'"
```
Post this go to `http://localhost:8888` and you will see a .zip file by the name of `processed_images.zip` which you can download and it contains the processed images which you can use for inferencing in the CLAY Foundation Model


## 🛠️ Management Commands

### Start/Stop the Container

```bash
# Start the container
docker-compose up -d

# Stop the container
docker-compose down

# Restart the container
docker-compose restart
```

### View Logs and Status

```bash
# Check if container is running
docker ps

# View container logs
docker-compose logs -f

# View real-time logs
docker-compose logs -f --tail=100
```

### Clean Up

```bash
# Stop and remove containers
docker-compose down

# Remove all unused Docker resources
docker system prune -f

# Remove specific images
docker rmi data-loader_clay_data-loader
```

## 🔧 Troubleshooting

### Common Issues and Solutions

#### 1. "Permission Denied" Error

```bash
# Make sure your user is in the docker group
groups $USER

# If docker is not listed, add it again
sudo usermod -aG docker $USER
newgrp docker
```

#### 2. "Port 8888 Already in Use"

```bash
# Find what's using port 8888
sudo lsof -i :8888

# Kill the process
sudo kill -9 <PID>

# Or stop all containers and restart
docker-compose down
docker-compose up -d
```

#### 3. "Container Won't Start"

```bash
# Check container logs
docker-compose logs

# Rebuild from scratch
docker-compose down
docker-compose build --no-cache
docker-compose up -d
```

#### 4. "Can't Access localhost:8888"

```bash
# Verify container is running
docker ps

# Check if port is exposed
docker port data-loader-clay

# Restart the container
docker-compose restart
```

### Docker Service Issues

```bash
# Check Docker service status
sudo systemctl status docker

# Start Docker service
sudo systemctl start docker

# Restart Docker service
sudo systemctl restart docker
```

## 📁 Project Structure

```
DATA-LOADER_CLAY/
├── Dockerfile              # Docker image configuration
├── docker-compose.yml      # Container orchestration
├── requirements.txt        # Python dependencies
├── data_process.ipynb     # Main Jupyter notebook
├── README.md              # This file
└── .dockerignore          # Files to exclude from Docker build
```

## 🔍 What's Inside the Container

- **Python 3.11** with scientific computing packages
- **Jupyter Notebook/Lab** for interactive development
- **GDAL** for geospatial data processing
- **NumPy, Matplotlib, PIL** for image processing
- **All dependencies** from requirements.txt

## 📚 Additional Resources

- [Docker Documentation](https://docs.docker.com/)
- [Jupyter Notebook Guide](https://jupyter-notebook.readthedocs.io/)
- [GDAL Documentation](https://gdal.org/)

## 🤝 Support

If you encounter issues:

1. **Check the troubleshooting section above**
2. **View container logs:** `docker-compose logs`
3. **Verify Docker installation:** `docker --version`
4. **Check system resources:** `docker system df`

## 📝 Notes

- **First build** takes 5 minutes due to downloading base images
- **Subsequent builds** are much faster due to Docker layer caching
- **Images are stored** in a Docker volume and persist between container restarts
- **To completely reset**, use `docker-compose down` and `docker system prune -f`

---

**Happy Processing! 🚀**

Your data processing environment is now ready. Open the notebook, run your code, and download your processed images!

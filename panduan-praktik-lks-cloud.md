# Panduan Praktik LKS Cloud Computing

## Modul 1: Linux System Administration

### 1. Manajemen User dan Permission
```bash
# Membuat user baru
sudo useradd username
sudo passwd username

# Membuat group
sudo groupadd groupname

# Menambahkan user ke group
sudo usermod -aG groupname username

# Mengatur permission
chmod 755 file.txt
chown user:group file.txt

# Melihat permission
ls -l file.txt
```

### 2. Package Management
```bash
# Update repository
sudo apt update
sudo apt upgrade

# Install package
sudo apt install package-name

# Remove package
sudo apt remove package-name

# Search package
apt search keyword
```

### 3. Service Management
```bash
# Start service
sudo systemctl start service-name

# Stop service
sudo systemctl stop service-name

# Enable service at boot
sudo systemctl enable service-name

# Check service status
sudo systemctl status service-name
```

### 4. Network Configuration
```bash
# Lihat network interface
ip addr show

# Set IP statis
sudo nano /etc/netplan/00-installer-config.yaml
# Contoh konfigurasi:
network:
  version: 2
  ethernets:
    ens33:
      addresses: [192.168.1.100/24]
      gateway4: 192.168.1.1
      nameservers:
        addresses: [8.8.8.8, 8.8.4.4]

# Apply konfigurasi
sudo netplan apply
```

## Modul 2: Web Server Setup

### 1. Apache Installation
```bash
# Install Apache
sudo apt install apache2

# Start Apache
sudo systemctl start apache2

# Enable Apache
sudo systemctl enable apache2

# Check status
sudo systemctl status apache2
```

### 2. PHP Setup
```bash
# Install PHP dan module
sudo apt install php php-mysql php-curl php-gd php-mbstring php-xml php-xmlrpc

# Verify PHP installation
php -v

# Test PHP
echo "<?php phpinfo(); ?>" | sudo tee /var/www/html/info.php
```

### 3. MySQL Installation
```bash
# Install MySQL
sudo apt install mysql-server

# Secure installation
sudo mysql_secure_installation

# Create database
mysql -u root -p
CREATE DATABASE dbname;
CREATE USER 'username'@'localhost' IDENTIFIED BY 'password';
GRANT ALL PRIVILEGES ON dbname.* TO 'username'@'localhost';
FLUSH PRIVILEGES;
```

## Modul 3: Docker Implementation

### 1. Docker Installation
```bash
# Install dependencies
sudo apt install apt-transport-https ca-certificates curl software-properties-common

# Add Docker repo
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"

# Install Docker
sudo apt update
sudo apt install docker-ce

# Start Docker
sudo systemctl start docker
sudo systemctl enable docker
```

### 2. Basic Docker Commands
```bash
# Pull image
docker pull nginx

# Run container
docker run -d -p 80:80 nginx

# List containers
docker ps

# Stop container
docker stop container_id

# Remove container
docker rm container_id

# List images
docker images
```

### 3. Docker Compose
```bash
# Install Docker Compose
sudo curl -L "https://github.com/docker/compose/releases/download/latest/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
sudo chmod +x /usr/local/bin/docker-compose

# Example docker-compose.yml
version: '3'
services:
  web:
    image: nginx
    ports:
      - "80:80"
  db:
    image: mysql:5.7
    environment:
      MYSQL_ROOT_PASSWORD: rootpass
      MYSQL_DATABASE: mydb
```

## Modul 4: AWS CLI dan Basic Services

### 1. AWS CLI Installation
```bash
# Install AWS CLI
curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
unzip awscliv2.zip
sudo ./aws/install

# Configure AWS CLI
aws configure
# Masukkan:
# AWS Access Key ID
# AWS Secret Access Key
# Default region name
# Default output format
```

### 2. EC2 Management
```bash
# Create key pair
aws ec2 create-key-pair --key-name MyKeyPair --query 'KeyMaterial' --output text > MyKeyPair.pem

# Launch instance
aws ec2 run-instances \
    --image-id ami-xxxxxxxx \
    --instance-type t2.micro \
    --key-name MyKeyPair \
    --security-group-ids sg-xxxxxxxx \
    --subnet-id subnet-xxxxxxxx

# List instances
aws ec2 describe-instances

# Stop instance
aws ec2 stop-instances --instance-ids i-xxxxxxxxxxxxxxxxx

# Terminate instance
aws ec2 terminate-instances --instance-ids i-xxxxxxxxxxxxxxxxx
```

### 3. S3 Management
```bash
# Create bucket
aws s3 mb s3://my-bucket-name

# Upload file
aws s3 cp file.txt s3://my-bucket-name/

# List objects
aws s3 ls s3://my-bucket-name/

# Download file
aws s3 cp s3://my-bucket-name/file.txt ./

# Delete object
aws s3 rm s3://my-bucket-name/file.txt

# Delete bucket
aws s3 rb s3://my-bucket-name --force
```

## Modul 5: Security Implementation

### 1. Firewall Configuration (UFW)
```bash
# Install UFW
sudo apt install ufw

# Enable UFW
sudo ufw enable

# Allow SSH
sudo ufw allow 22

# Allow HTTP/HTTPS
sudo ufw allow 80
sudo ufw allow 443

# Check status
sudo ufw status
```

### 2. SSL/TLS Setup
```bash
# Install Certbot
sudo apt install certbot python3-certbot-apache

# Get certificate
sudo certbot --apache -d example.com

# Auto-renewal
sudo certbot renew --dry-run
```

### 3. Security Groups (AWS)
```bash
# Create security group
aws ec2 create-security-group \
    --group-name MySecurityGroup \
    --description "My security group"

# Add inbound rule
aws ec2 authorize-security-group-ingress \
    --group-id sg-xxxxxxxx \
    --protocol tcp \
    --port 22 \
    --cidr 0.0.0.0/0
```

## Tips Penting
1. Selalu backup data sebelum melakukan perubahan besar
2. Dokumentasikan setiap perubahan konfigurasi
3. Test konfigurasi di environment development sebelum production
4. Monitor log untuk troubleshooting
5. Gunakan strong password dan key management
6. Update sistem secara berkala

## Monitoring Commands
```bash
# System monitoring
top
htop
df -h
free -m
netstat -tulpn

# Log monitoring
tail -f /var/log/syslog
journalctl -f

# Service monitoring
sudo systemctl status service-name
docker stats
aws cloudwatch get-metric-statistics
```

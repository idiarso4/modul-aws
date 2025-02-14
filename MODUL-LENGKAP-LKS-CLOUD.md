# MODUL LENGKAP PERSIAPAN LKS CLOUD COMPUTING

## Daftar Isi
1. [Pendahuluan](#1-pendahuluan)
2. [Persiapan Dasar](#2-persiapan-dasar)
3. [Fundamental Linux dan Server](#3-fundamental-linux-dan-server)
4. [AWS Core Services](#4-aws-core-services)
5. [Networking dan Security](#5-networking-dan-security)
6. [Database dan Storage](#6-database-dan-storage)
7. [Application Development](#7-application-development)
8. [Monitoring dan Troubleshooting](#8-monitoring-dan-troubleshooting)
9. [Kriteria Penilaian](#9-kriteria-penilaian)

## 1. Pendahuluan

### 1.1 Tentang LKS Cloud Computing
Lomba Kompetensi Siswa (LKS) bidang Cloud Computing menguji kemampuan peserta dalam:
- Pengelolaan infrastruktur cloud
- Konfigurasi jaringan dan keamanan
- Deployment aplikasi
- Troubleshooting
- Manajemen waktu

### 1.2 Persiapan Peserta
- Laptop dengan spesifikasi memadai
- Koneksi internet stabil
- AWS Free Tier Account
- Text editor
- Terminal emulator
- Browser modern

## 2. Persiapan Dasar

### 2.1 Setup Environment
```bash
# Install AWS CLI
curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
unzip awscliv2.zip
sudo ./aws/install

# Konfigurasi AWS CLI
aws configure
# Masukkan:
# - AWS Access Key ID
# - AWS Secret Access Key
# - Default region
# - Output format
```

### 2.2 Tools yang Diperlukan
```bash
# Install tools dasar
sudo apt update
sudo apt install -y \
    git \
    curl \
    wget \
    unzip \
    net-tools \
    htop \
    vim
```

## 3. Fundamental Linux dan Server

### 3.1 Linux System Administration
```bash
# User Management
sudo useradd username
sudo passwd username
sudo usermod -aG sudo username

# File Permissions
chmod 755 file.txt
chown user:group file.txt

# Service Management
sudo systemctl start service-name
sudo systemctl enable service-name
sudo systemctl status service-name
```

### 3.2 Web Server Setup

#### Apache Installation
```bash
# Ubuntu/Debian
sudo apt install apache2
sudo systemctl start apache2
sudo systemctl enable apache2

# CentOS/RHEL
sudo yum install httpd
sudo systemctl start httpd
sudo systemctl enable httpd
```

#### Nginx Installation
```bash
sudo apt install nginx
sudo systemctl start nginx
sudo systemctl enable nginx
```

## 4. AWS Core Services

### 4.1 Virtual Private Cloud (VPC)
```bash
# Create VPC
aws ec2 create-vpc \
    --cidr-block 10.0.0.0/16 \
    --tag-specifications 'ResourceType=vpc,Tags=[{Key=Name,Value=MyVPC}]'

# Enable DNS hostnames
aws ec2 modify-vpc-attribute \
    --vpc-id vpc-xxx \
    --enable-dns-hostnames "{\"Value\":true}"
```

### 4.2 Subnet Configuration
```bash
# Create Public Subnet
aws ec2 create-subnet \
    --vpc-id vpc-xxx \
    --cidr-block 10.0.1.0/24 \
    --availability-zone us-east-1a

# Create Private Subnet
aws ec2 create-subnet \
    --vpc-id vpc-xxx \
    --cidr-block 10.0.2.0/24 \
    --availability-zone us-east-1a
```

### 4.3 Route Tables
```bash
# Create Route Table
aws ec2 create-route-table --vpc-id vpc-xxx

# Add Internet Gateway Route
aws ec2 create-route \
    --route-table-id rtb-xxx \
    --destination-cidr-block 0.0.0.0/0 \
    --gateway-id igw-xxx
```

### 4.4 Internet & NAT Gateway
```bash
# Create and Attach Internet Gateway
aws ec2 create-internet-gateway
aws ec2 attach-internet-gateway \
    --vpc-id vpc-xxx \
    --internet-gateway-id igw-xxx

# Create NAT Gateway
aws ec2 create-nat-gateway \
    --subnet-id subnet-xxx \
    --allocation-id eipalloc-xxx
```

## 5. Networking dan Security

### 5.1 Security Groups
```bash
# Create Security Group
aws ec2 create-security-group \
    --group-name MySecurityGroup \
    --description "My security group" \
    --vpc-id vpc-xxx

# Add Inbound Rules
aws ec2 authorize-security-group-ingress \
    --group-id sg-xxx \
    --protocol tcp \
    --port 80 \
    --cidr 0.0.0.0/0
```

### 5.2 IAM Configuration
```bash
# Create IAM User
aws iam create-user --user-name MyUser

# Create Access Key
aws iam create-access-key --user-name MyUser

# Create IAM Role
aws iam create-role \
    --role-name MyRole \
    --assume-role-policy-document file://trust-policy.json
```

### 5.3 Key Pairs
```bash
# Create Key Pair
aws ec2 create-key-pair \
    --key-name MyKeyPair \
    --query 'KeyMaterial' \
    --output text > MyKeyPair.pem

chmod 400 MyKeyPair.pem
```

## 6. Database dan Storage

### 6.1 RDS Setup
```bash
# Create RDS Instance
aws rds create-db-instance \
    --db-instance-identifier mydb \
    --db-instance-class db.t3.micro \
    --engine mysql \
    --master-username admin \
    --master-user-password password123 \
    --allocated-storage 20
```

### 6.2 S3 Configuration
```bash
# Create Bucket
aws s3 mb s3://my-unique-bucket-name

# Upload File
aws s3 cp file.txt s3://my-bucket/

# Configure Static Website
aws s3 website s3://my-bucket/ \
    --index-document index.html \
    --error-document error.html
```

## 7. Application Development

### 7.1 WordPress Installation dan Konfigurasi

#### A. Persiapan Server
```bash
# Update sistem
sudo apt update && sudo apt upgrade -y

# Install LAMP Stack
sudo apt install apache2 mysql-server php php-mysql php-curl php-gd php-mbstring php-xml php-xmlrpc php-soap php-intl php-zip -y

# Start dan enable services
sudo systemctl start apache2
sudo systemctl enable apache2
sudo systemctl start mysql
sudo systemctl enable mysql
```

#### B. Konfigurasi MySQL
```bash
# Secure MySQL installation
sudo mysql_secure_installation

# Buat database WordPress
sudo mysql -u root -p
CREATE DATABASE wordpress;
CREATE USER 'wpuser'@'localhost' IDENTIFIED BY 'password_kuat';
GRANT ALL PRIVILEGES ON wordpress.* TO 'wpuser'@'localhost';
FLUSH PRIVILEGES;
EXIT;
```

#### C. Install WordPress
```bash
# Download WordPress
cd /tmp
wget https://wordpress.org/latest.tar.gz
tar xzvf latest.tar.gz

# Pindahkan ke web root
sudo mv wordpress /var/www/html/

# Set permissions
sudo chown -R www-data:www-data /var/www/html/wordpress
sudo chmod -R 755 /var/www/html/wordpress
```

#### D. Konfigurasi Apache Virtual Host
```bash
# Buat file konfigurasi
sudo nano /etc/apache2/sites-available/wordpress.conf

# Isi dengan konfigurasi berikut
<VirtualHost *:80>
    ServerAdmin webmaster@localhost
    DocumentRoot /var/www/html/wordpress
    ServerName example.com
    ServerAlias www.example.com

    <Directory /var/www/html/wordpress/>
        Options FollowSymLinks
        AllowOverride All
        Require all granted
    </Directory>

    ErrorLog ${APACHE_LOG_DIR}/wordpress_error.log
    CustomLog ${APACHE_LOG_DIR}/wordpress_access.log combined
</VirtualHost>

# Enable site dan module
sudo a2ensite wordpress.conf
sudo a2enmod rewrite
sudo systemctl restart apache2
```

#### E. Konfigurasi wp-config.php
```bash
# Copy sample config
cd /var/www/html/wordpress
sudo cp wp-config-sample.php wp-config.php

# Edit konfigurasi
sudo nano wp-config.php

# Tambahkan konfigurasi berikut
define('DB_NAME', 'wordpress');
define('DB_USER', 'wpuser');
define('DB_PASSWORD', 'password_kuat');
define('DB_HOST', 'localhost');
define('DB_CHARSET', 'utf8');
define('DB_COLLATE', '');

# Tambahkan salt keys (generate dari https://api.wordpress.org/secret-key/1.1/salt/)
define('AUTH_KEY', 'your-unique-key');
define('SECURE_AUTH_KEY', 'your-unique-key');
define('LOGGED_IN_KEY', 'your-unique-key');
define('NONCE_KEY', 'your-unique-key');
define('AUTH_SALT', 'your-unique-key');
define('SECURE_AUTH_SALT', 'your-unique-key');
define('LOGGED_IN_SALT', 'your-unique-key');
define('NONCE_SALT', 'your-unique-key');
```

#### F. Optimasi WordPress
```bash
# Edit wp-config.php untuk performance
define('WP_MEMORY_LIMIT', '256M');
define('WP_MAX_MEMORY_LIMIT', '256M');
define('WP_POST_REVISIONS', 5);
define('AUTOSAVE_INTERVAL', 300);
define('WP_DEBUG', false);

# Tambahkan di .htaccess untuk caching
<IfModule mod_expires.c>
    ExpiresActive On
    ExpiresByType image/jpg "access plus 1 year"
    ExpiresByType image/jpeg "access plus 1 year"
    ExpiresByType image/gif "access plus 1 year"
    ExpiresByType image/png "access plus 1 year"
    ExpiresByType text/css "access plus 1 month"
    ExpiresByType application/javascript "access plus 1 month"
</IfModule>
```

#### G. Keamanan WordPress
```bash
# Proteksi wp-config.php
sudo chmod 600 wp-config.php

# Tambahkan di wp-config.php
define('DISALLOW_FILE_EDIT', true);
define('WP_AUTO_UPDATE_CORE', true);

# Proteksi direktori
sudo find /var/www/html/wordpress -type d -exec chmod 755 {} \;
sudo find /var/www/html/wordpress -type f -exec chmod 644 {} \;
```

#### H. Backup Setup
```bash
# Backup database
mysqldump -u root -p wordpress > wordpress_backup.sql

# Backup files
sudo tar -czf wordpress_files_backup.tar.gz /var/www/html/wordpress

# Buat script backup otomatis
sudo nano /root/backup-wordpress.sh

#!/bin/bash
TIMESTAMP=$(date +"%Y%m%d_%H%M%S")
BACKUP_DIR="/backup/wordpress"
mysqldump -u root -p'password' wordpress > $BACKUP_DIR/db_$TIMESTAMP.sql
tar -czf $BACKUP_DIR/files_$TIMESTAMP.tar.gz /var/www/html/wordpress

# Tambahkan ke crontab
0 0 * * * /root/backup-wordpress.sh
```

#### I. Monitoring
```bash
# Monitor Apache logs
tail -f /var/log/apache2/wordpress_error.log
tail -f /var/log/apache2/wordpress_access.log

# Monitor MySQL logs
tail -f /var/log/mysql/error.log

# Check PHP configuration
php -i | grep memory_limit
php -i | grep max_execution_time
```

### 7.2 Lambda Functions
```bash
# Create Lambda Function
aws lambda create-function \
    --function-name MyFunction \
    --runtime nodejs14.x \
    --role arn:aws:iam::ACCOUNT_ID:role/lambda-role \
    --handler index.handler \
    --zip-file fileb://function.zip
```

### 7.3 API Gateway
```bash
# Create REST API
aws apigateway create-rest-api \
    --name 'My API' \
    --description 'My REST API'

# Create Resource
aws apigateway create-resource \
    --rest-api-id xxx \
    --parent-id xxx \
    --path-part items
```

### 7.4 React.js Deployment
```bash
# Build React App
npm run build

# Deploy to S3
aws s3 sync build/ s3://my-bucket/

# Configure CloudFront
aws cloudfront create-distribution \
    --origin-domain-name my-bucket.s3.amazonaws.com
```

## 8. Monitoring dan Troubleshooting

### 8.1 CloudWatch
```bash
# Create Alarm
aws cloudwatch put-metric-alarm \
    --alarm-name cpu-mon \
    --alarm-description "CPU utilization" \
    --metric-name CPUUtilization \
    --namespace AWS/EC2 \
    --statistic Average \
    --period 300 \
    --threshold 70 \
    --comparison-operator GreaterThanThreshold \
    --evaluation-periods 2

# Get Metrics
aws cloudwatch get-metric-statistics \
    --namespace AWS/EC2 \
    --metric-name CPUUtilization \
    --dimensions Name=InstanceId,Value=i-xxx \
    --start-time 2024-02-14T00:00:00 \
    --end-time 2024-02-14T23:59:59 \
    --period 3600 \
    --statistics Average
```

### 8.2 Logging
```bash
# View CloudWatch Logs
aws logs get-log-events \
    --log-group-name /aws/lambda/MyFunction \
    --log-stream-name 2024/02/14

# Create Log Group
aws logs create-log-group \
    --log-group-name /my/application
```

## 9. Kriteria Penilaian

### 9.1 Komponen Penilaian
1. Pengelolaan personal (10 poin)
2. Penguasaan teori (10 poin)
3. Keberhasilan menyelesaikan tugas:
   - Virtual Private Cloud (5 poin)
   - Subnet & VPC (5 poin)
   - Route Tables (10 poin)
   - Internet gateway (5 poin)
   - NAT gateway (5 poin)
   - Endpoint (5 poin)
   - Key Pairs (5 poin)
   - IAM (10 poin)
   - Security Groups (10 poin)
   - Databases (5 poin)
   - AWS Lambda (5 poin)
   - API Gateway (10 poin)
   - Application Deployment (10 poin)
   - React.js apps (5 poin)
   - High Availability (5 poin)
4. Troubleshooting (10 poin)
5. Waktu pengerjaan (10 poin)

### 9.2 Tips Menghadapi Lomba
1. Baca soal dengan teliti
2. Prioritaskan komponen dengan bobot nilai tinggi
3. Manajemen waktu yang baik
4. Dokumentasikan setiap langkah
5. Verifikasi konfigurasi
6. Backup sebelum perubahan besar
7. Gunakan template dan script yang sudah disiapkan

### 9.3 Checklist Persiapan
- [ ] AWS Account aktif
- [ ] Tools terinstall dan terkonfigurasi
- [ ] Template dan script siap
- [ ] Dokumentasi offline tersedia
- [ ] Latihan timeboxing
- [ ] Backup plan siap

## Referensi
1. [AWS Documentation](https://docs.aws.amazon.com)
2. [AWS CLI Command Reference](https://awscli.amazonaws.com/v2/documentation/api/latest/index.html)
3. [AWS Best Practices](https://aws.amazon.com/architecture/well-architected/)
4. [React Documentation](https://reactjs.org/docs)
5. [Linux Documentation](https://www.kernel.org/doc/)

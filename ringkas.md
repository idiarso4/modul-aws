# Ringkasan Perintah untuk AWS Cloud Computing

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
# Install AWS CLI on Windows
1. Unduh Installer AWS CLI:
   Anda dapat mengunduh installer AWS CLI MSI untuk Windows dari tautan berikut:
   [AWS CLI MSI Installer](https://awscli.amazonaws.com/AWSCLIV2.msi)

2. Jalankan Installer:
   - Klik dua kali pada file `.msi` yang telah diunduh dan ikuti instruksi di layar untuk menyelesaikan instalasi.

3. Verifikasi Instalasi:
   Setelah instalasi, Anda dapat memverifikasi bahwa AWS CLI telah terinstal dengan benar dengan membuka Command Prompt dan menjalankan:
   ```bash
   aws --version
   ```

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
# Install tools dasar untuk Windows
1. Git: Versi kontrol untuk mengelola kode sumber. Unduh dari [sini](https://git-scm.com/download/win).
2. Curl: Alat untuk mentransfer data dengan URL. Sudah termasuk di Windows 10 dan versi yang lebih baru.
3. Visual Studio Code: Editor kode sumber yang populer. Unduh dari [sini](https://code.visualstudio.com/download).
4. PuTTY: Klien SSH untuk mengakses server. Unduh dari [sini](https://www.putty.org/).
5. FileZilla: Klien FTP untuk mengelola file. Unduh dari [sini](https://filezilla-project.org/download.php).
```

## 3. Fundamental Linux dan Server

### 3.1 Pembuatan User Baru di AWS dan Kunci Menggunakan PuTTY

1. **Pembuatan User Baru di AWS**:
   - Masuk ke AWS Management Console.
   - Navigasi ke IAM (Identity and Access Management).
   - Klik pada Users dan kemudian Add user.
   - Masukkan nama pengguna dan pilih jenis akses (Access key atau AWS Management Console access).
   - Klik Next: Permissions untuk mengatur izin pengguna.
   - Setelah selesai, klik Create user. Catat Access Key ID dan Secret Access Key jika dibuat.

2. **Pembuatan Kunci Menggunakan PuTTY di Windows**:
   - Unduh dan instal PuTTY dan PuTTYgen dari [sini](https://www.putty.org/).
   - Buka PuTTYgen dan pilih RSA sebagai tipe kunci.
   - Atur Number of bits in a generated key ke 2048.
   - Klik Generate dan gerakkan mouse Anda di area kosong untuk menghasilkan kunci.
   - Setelah kunci dibuat, simpan Public key dan Private key (dalam format `.ppk`).
   - Anda dapat menggunakan Public key untuk mengonfigurasi akses SSH ke instance AWS Anda.

### 3.2 Web Server Setup

#### Apache Installation

```bash
# Instalasi Apache di AWS EC2 (Ubuntu/Debian)
1. **Masuk ke Instance EC2**:
   - Gunakan SSH untuk terhubung ke instance EC2 Anda. Contoh perintah:
     ```bash
     ssh -i /path/to/your-key.pem ubuntu@your-ec2-public-ip
     ```

2. **Perbarui Daftar Paket**:
   - Sebelum menginstal Apache, pastikan daftar paket Anda diperbarui dengan menjalankan:
     ```bash
     sudo apt update
     ```

3. **Instal Apache**:
   - Jalankan perintah berikut untuk menginstal Apache:
     ```bash
     sudo apt install apache2
     ```

4. **Mulai dan Aktifkan Apache**:
   - Setelah instalasi selesai, mulai layanan Apache dan atur agar otomatis berjalan saat booting:
     ```bash
     sudo systemctl start apache2
     sudo systemctl enable apache2
     ```

5. **Verifikasi Instalasi**:
   - Untuk memastikan Apache telah terinstal dan berjalan dengan baik, buka browser dan masukkan alamat IP publik instance EC2 Anda. Anda seharusnya melihat halaman default Apache.

6. **Konfigurasi Firewall**:
   - Pastikan port 80 (HTTP) dan 443 (HTTPS) dibuka di grup keamanan AWS Anda agar dapat diakses dari internet.

Dengan mengikuti langkah-langkah di atas, Anda akan berhasil menginstal dan mengonfigurasi Apache di instance AWS EC2 Anda.

### 3.3 Linux System Administration

```bash
# User Management
# Membuat pengguna baru di sistem
sudo useradd username
# Mengatur kata sandi untuk pengguna baru
sudo passwd username
# Menambahkan pengguna ke grup sudo untuk memberikan izin administratif
sudo usermod -aG sudo username

# File Permissions
# Mengatur izin file sehingga pemilik dapat membaca, menulis, dan mengeksekusi,
# sementara grup dan lainnya hanya dapat membaca dan mengeksekusi
chmod 755 file.txt
# Mengubah pemilik dan grup dari file
chown user:group file.txt

# Service Management
# Memulai layanan
sudo systemctl start service-name
# Mengatur layanan agar otomatis dimulai saat booting
sudo systemctl enable service-name
# Memeriksa status layanan
sudo systemctl status service-name

# Catatan:
# Bagian ini penting untuk manajemen instance EC2 berbasis Linux di AWS.
# Pengelolaan pengguna dan izin file membantu menjaga keamanan sistem,
# sedangkan manajemen layanan memungkinkan Anda untuk mengontrol aplikasi yang berjalan di server.
```

### 3.4 Pembuatan Pengguna Baru di AWS EC2

Untuk membuat pengguna baru dengan nama `nafis`, menambahkan pengguna tersebut ke grup `sija`, dan memastikan pengguna tersebut dapat terhubung ke instance EC2 tanpa menggunakan akun root, ikuti langkah-langkah berikut:

1. **Masuk ke Instance EC2**:
   Pertama, terhubung ke instance EC2 Anda menggunakan SSH dengan akun root atau akun yang sudah memiliki izin sudo.
   ```bash
   ssh -i /path/to/your-key.pem ubuntu@175.165.1.52
   ```

2. **Buat Pengguna Baru**:
   Setelah terhubung, jalankan perintah berikut untuk membuat pengguna baru dengan nama `nafis`.
   ```bash
   sudo useradd nafis
   ```

3. **Atur Kata Sandi untuk Pengguna Baru**:
   Selanjutnya, atur kata sandi untuk pengguna `nafis`.
   ```bash
   sudo passwd nafis
   ```

4. **Tambahkan Pengguna ke Grup `sija`**:
   Jika grup `sija` sudah ada, Anda dapat menambahkan pengguna `nafis` ke grup tersebut.
   ```bash
   sudo usermod -aG sija nafis
   ```

5. **Verifikasi Pengguna dan Grup**:
   Anda dapat memverifikasi bahwa pengguna `nafis` telah ditambahkan ke grup `sija` dengan menjalankan perintah berikut:
   ```bash
   groups nafis
   ```

6. **Konfigurasi SSH (Opsional)**:
   Jika Anda ingin pengguna `nafis` dapat terhubung ke instance EC2 melalui SSH, Anda perlu menambahkan kunci publik SSH pengguna tersebut ke file `~/.ssh/authorized_keys` di home directory pengguna `nafis`. Anda dapat menggunakan perintah berikut untuk melakukannya:
   ```bash
   sudo mkdir /home/nafis/.ssh
   sudo touch /home/nafis/.ssh/authorized_keys
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
    --enable-dns-hostnames '{"Value":true}'
```

**Contoh Alamat IP**: `10.0.0.0/16` - Rentang alamat IP untuk VPC yang baru dibuat.

### 4.2 Konfigurasi Subnet

```bash
# Buat Subnet Publik
aws ec2 create-subnet \
    --vpc-id vpc-xxx \
    --cidr-block 10.0.1.0/24 \
    --availability-zone us-east-1a

# Buat Subnet Privat
aws ec2 create-subnet \
    --vpc-id vpc-xxx \
    --cidr-block 10.0.2.0/24 \
    --availability-zone us-east-1a
```

- **Membuat Subnet Publik**: Perintah ini digunakan untuk membuat subnet publik dalam VPC yang telah dibuat. Subnet ini menggunakan rentang alamat IP `10.0.1.0/24` dan berada di zona ketersediaan `us-east-1a`.
- **Membuat Subnet Privat**: Perintah ini digunakan untuk membuat subnet privat dengan rentang alamat IP `10.0.2.0/24`, juga di zona ketersediaan yang sama.

### 4.3 Tabel Rute

```bash
# Buat Tabel Rute
aws ec2 create-route-table --vpc-id vpc-xxx

# Tambahkan Rute Internet Gateway
aws ec2 create-route \
    --route-table-id rtb-xxx \
    --destination-cidr-block 0.0.0.0/0 \
    --gateway-id igw-xxx
```

- **Membuat Tabel Rute**: Perintah ini digunakan untuk membuat tabel rute baru dalam VPC. Tabel rute digunakan untuk menentukan bagaimana paket data dikirim ke alamat IP tertentu.
- **Menambahkan Rute Internet Gateway**: Perintah ini menambahkan rute ke tabel rute yang mengarahkan semua lalu lintas (0.0.0.0/0) ke Internet Gateway yang telah dibuat.

### 4.4 Internet & NAT Gateway

```bash
# Buat dan Lampirkan Internet Gateway
aws ec2 create-internet-gateway
aws ec2 attach-internet-gateway \
    --vpc-id vpc-xxx \
    --internet-gateway-id igw-xxx

# Buat NAT Gateway
aws ec2 create-nat-gateway \
    --subnet-id subnet-xxx \
    --allocation-id eipalloc-xxx
```

- **Membuat dan Melampirkan Internet Gateway**: Perintah pertama digunakan untuk membuat Internet Gateway, yang memungkinkan komunikasi antara VPC dan internet. Perintah kedua melampirkan Internet Gateway tersebut ke VPC.
- **Membuat NAT Gateway**: NAT Gateway digunakan untuk memungkinkan instance di subnet privat untuk mengakses internet tanpa memberikan akses langsung dari internet ke instance tersebut.

## 5. Networking dan Security

### 5.1 Security Groups
```bash
# Buat Security Group
aws ec2 create-security-group \
    --group-name MySecurityGroup \
    --description "My security group" \
    --vpc-id vpc-xxx

# Tambahkan Aturan Inbound
aws ec2 authorize-security-group-ingress \
    --group-id sg-xxx \
    --protocol tcp \
    --port 80 \
    --cidr 0.0.0.0/0
```

**1. Membuat Security Group**:
- **Perintah**: `aws ec2 create-security-group` digunakan untuk membuat grup keamanan baru di AWS.
- **Parameter**:
  - `--group-name MySecurityGroup`: Menentukan nama untuk grup keamanan yang baru dibuat.
  - `--description "My security group"`: Memberikan deskripsi untuk grup keamanan tersebut.
  - `--vpc-id vpc-xxx`: Menentukan ID VPC tempat grup keamanan ini akan diterapkan.

**2. Menambahkan Aturan Inbound**:
- **Perintah**: `aws ec2 authorize-security-group-ingress` digunakan untuk menambahkan aturan inbound ke grup keamanan.
- **Parameter**:
  - `--group-id sg-xxx`: Menentukan ID grup keamanan yang akan dimodifikasi.
  - `--protocol tcp`: Menentukan protokol yang akan digunakan.
  - `--port 80`: Menentukan port yang akan dibuka.
  - `--cidr 0.0.0.0/0`: Menentukan rentang alamat IP yang akan diizinkan mengakses port ini.

### 5.2 Konfigurasi IAM

```bash
# Buat Pengguna IAM
aws iam create-user --user-name nafis

# Buat Kunci Akses
aws iam create-access-key --user-name nafis

# Buat Peran IAM
aws iam create-role \
    --role-name MyRole \
    --assume-role-policy-document file://trust-policy.json
```

**1. Membuat Pengguna IAM**:
- **Perintah**: `aws iam create-user --user-name nafis`
  - Perintah ini digunakan untuk membuat pengguna baru di AWS IAM dengan nama `nafis`. Pengguna IAM dapat digunakan untuk mengelola akses ke layanan AWS.

**2. Membuat Kunci Akses**:
- **Perintah**: `aws iam create-access-key --user-name nafis`
  - Setelah pengguna `nafis` dibuat, perintah ini digunakan untuk membuat kunci akses yang memungkinkan pengguna tersebut untuk mengakses AWS melalui API. Kunci akses terdiri dari Access Key ID dan Secret Access Key.

**3. Membuat Peran IAM**:
- **Perintah**: `aws iam create-role --role-name MyRole --assume-role-policy-document file://trust-policy.json`
  - Perintah ini digunakan untuk membuat peran IAM baru dengan nama `MyRole`. Peran ini dapat digunakan oleh layanan AWS atau pengguna lain untuk mendapatkan izin tertentu. `trust-policy.json` adalah file yang berisi kebijakan yang menentukan siapa yang dapat menggunakan peran ini.

### 5.3 Key Pairs

```bash
# Buat Key Pair
aws ec2 create-key-pair \
    --key-name MyKeyPair \
    --query 'KeyMaterial' \
    --output text > MyKeyPair.pem

chmod 400 MyKeyPair.pem
```

**1. Membuat Key Pair**:
- **Perintah**: `aws ec2 create-key-pair` digunakan untuk membuat key pair yang akan digunakan untuk mengakses instance EC2.
- **Parameter**:
  - `--key-name MyKeyPair`: Menentukan nama untuk key pair yang akan dibuat.
  - `--query 'KeyMaterial'`: Mengambil material kunci dari output.
  - `--output text`: Mengatur format output menjadi teks.
- **Output**: Hasil dari perintah ini akan disimpan dalam file `MyKeyPair.pem`, yang berisi private key untuk mengakses instance EC2.

**2. Mengatur Izin File**:
- **Perintah**: `chmod 400 MyKeyPair.pem`
  - Perintah ini digunakan untuk mengatur izin file sehingga hanya pemilik yang dapat membacanya.

### 6.1 Pengaturan RDS

```bash
# Buat Instance RDS
aws rds create-db-instance \
    --db-instance-identifier mydb \
    --db-instance-class db.t3.micro \
    --engine mysql \
    --master-username admin \
    --master-user-password password123 \
    --allocated-storage 20
```

**1. Membuat Instance RDS**:
- **Perintah**: `aws rds create-db-instance` digunakan untuk membuat instance database RDS.
- **Parameter**:
  - `--db-instance-identifier mydb`: Menentukan identifier untuk instance database.
  - `--db-instance-class db.t3.micro`: Menentukan kelas instance untuk database.
  - `--engine mysql`: Menentukan jenis database yang akan digunakan.
  - `--master-username admin`: Menentukan nama pengguna master untuk database.
  - `--master-user-password password123`: Menentukan kata sandi untuk pengguna master.
  - `--allocated-storage 20`: Menentukan jumlah penyimpanan yang dialokasikan untuk database.

### 6.2 Konfigurasi S3

```bash
# Buat Bucket
aws s3 mb s3://my-unique-bucket-name

# Unggah File
aws s3 cp file.txt s3://my-bucket/

# Konfigurasi Website Statik
aws s3 website s3://my-bucket/ \
    --index-document index.html \
    --error-document error.html
```

**1. Membuat Bucket**:
- **Perintah**: `aws s3 mb s3://my-unique-bucket-name`
  - Perintah ini digunakan untuk membuat bucket S3 baru.

**2. Mengunggah File**:
- **Perintah**: `aws s3 cp file.txt s3://my-bucket/`
  - Perintah ini digunakan untuk mengunggah file `file.txt` ke bucket S3 yang telah dibuat.

**3. Mengonfigurasi Website Statik**:
- **Perintah**: `aws s3 website s3://my-bucket/ --index-document index.html --error-document error.html`
  - Perintah ini mengonfigurasi bucket S3 untuk digunakan sebagai website statik.

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

## Penyelarasan Pengguna Baru

### Host: 175.165.1.52

1. **Buat Pengguna Baru**:
   Untuk membuat pengguna baru dengan nama `nafis`, jalankan perintah berikut:
   ```bash
   sudo useradd nafis
   ```

2. **Atur Kata Sandi untuk Pengguna Baru**:
   Selanjutnya, atur kata sandi untuk pengguna `nafis`.
   ```bash
   sudo passwd nafis
   ```

3. **Tambahkan Pengguna ke Grup `sija`**:
   Jika grup `sija` sudah ada, Anda dapat menambahkan pengguna `nafis` ke grup tersebut.
   ```bash
   sudo usermod -aG sija nafis
   ```

4. **Verifikasi Pengguna dan Grup**:
   Anda dapat memverifikasi bahwa pengguna `nafis` telah ditambahkan ke grup `sija` dengan menjalankan perintah berikut:
   ```bash
   groups nafis
   ```
   
5. **Konfigurasi SSH (Opsional)**:
   Jika Anda ingin pengguna `nafis` dapat terhubung ke instance EC2 melalui SSH, Anda perlu menambahkan kunci publik SSH pengguna tersebut ke file `~/.ssh/authorized_keys` di home directory pengguna `nafis`. Anda dapat menggunakan perintah berikut untuk melakukannya:
   ```bash
   sudo mkdir /home/nafis/.ssh
   sudo touch /home/nafis/.ssh/authorized_keys
   ```
```
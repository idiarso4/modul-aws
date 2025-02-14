# Panduan Instalasi WordPress

## A. Instalasi Apache Server

### 1. Instalasi Apache di Ubuntu/Debian
```bash
# Update repository
sudo apt update
sudo apt upgrade

# Install Apache
sudo apt install apache2

# Start dan enable Apache
sudo systemctl start apache2
sudo systemctl enable apache2

# Cek status
sudo systemctl status apache2
```

### 2. Instalasi Apache di CentOS/RHEL
```bash
# Update system
sudo yum update

# Install Apache
sudo yum install httpd

# Start dan enable Apache
sudo systemctl start httpd
sudo systemctl enable httpd

# Cek status
sudo systemctl status httpd
```

### 3. Instalasi Apache di Windows
```powershell
# Download Apache dari Apache Lounge
# Ekstrak ke C:\Apache24

# Install service Apache
cd C:\Apache24\bin
httpd.exe -k install

# Start Apache service
net start Apache2.4
```

## B. Alternatif Web Server

### 1. Nginx
```bash
# Ubuntu/Debian
sudo apt install nginx
sudo systemctl start nginx
sudo systemctl enable nginx

# CentOS/RHEL
sudo yum install nginx
sudo systemctl start nginx
sudo systemctl enable nginx
```

### 2. LiteSpeed
```bash
# Download dari litespeedtech.com
wget https://openlitespeed.org/packages/openlitespeed-1.7.16.tgz
tar xzvf openlitespeed-1.7.16.tgz
cd openlitespeed
./install.sh
```

## C. Opsi Database Server

### 1. MySQL
```bash
# Ubuntu/Debian
sudo apt install mysql-server
sudo systemctl start mysql
sudo mysql_secure_installation

# Buat database WordPress
mysql -u root -p
CREATE DATABASE wordpress;
CREATE USER 'wpuser'@'localhost' IDENTIFIED BY 'password';
GRANT ALL PRIVILEGES ON wordpress.* TO 'wpuser'@'localhost';
FLUSH PRIVILEGES;
```

### 2. MariaDB
```bash
# Ubuntu/Debian
sudo apt install mariadb-server
sudo systemctl start mariadb
sudo mysql_secure_installation

# CentOS/RHEL
sudo yum install mariadb-server mariadb
sudo systemctl start mariadb
sudo mysql_secure_installation
```

### 3. PostgreSQL
```bash
# Install PostgreSQL
sudo apt install postgresql postgresql-contrib

# Buat user dan database
sudo -u postgres psql
CREATE DATABASE wordpress;
CREATE USER wpuser WITH PASSWORD 'password';
GRANT ALL PRIVILEGES ON DATABASE wordpress TO wpuser;
```

## D. Instalasi WordPress

### 1. Instalasi Manual
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

### 2. Instalasi via WP-CLI
```bash
# Install WP-CLI
curl -O https://raw.githubusercontent.com/wp-cli/builds/gh-pages/phar/wp-cli.phar
chmod +x wp-cli.phar
sudo mv wp-cli.phar /usr/local/bin/wp

# Install WordPress
cd /var/www/html
wp core download
wp core config --dbname=wordpress --dbuser=wpuser --dbpass=password
wp core install --url=example.com --title="WordPress Site" --admin_user=admin --admin_password=password --admin_email=admin@example.com
```

## E. Konfigurasi Dasar WordPress

### 1. Pengaturan Permalink
1. Login ke WordPress admin
2. Pergi ke Settings > Permalinks
3. Pilih struktur URL yang diinginkan (Post name recommended)
4. Simpan perubahan

### 2. Konfigurasi .htaccess
```apache
# File: /var/www/html/wordpress/.htaccess
<IfModule mod_rewrite.c>
RewriteEngine On
RewriteBase /
RewriteRule ^index\.php$ - [L]
RewriteCond %{REQUEST_FILENAME} !-f
RewriteCond %{REQUEST_FILENAME} !-d
RewriteRule . /index.php [L]
</IfModule>
```

### 3. Optimasi WordPress
```bash
# Edit wp-config.php
define('WP_POST_REVISIONS', 5);
define('AUTOSAVE_INTERVAL', 300);
define('WP_MEMORY_LIMIT', '256M');
define('WP_MAX_MEMORY_LIMIT', '256M');
```

### 4. Keamanan Dasar
```bash
# Proteksi wp-config.php
sudo chmod 600 wp-config.php

# Disable file editing di wp-config.php
define('DISALLOW_FILE_EDIT', true);

# Proteksi direktori
sudo chmod 755 /var/www/html/wordpress
```

## F. Monitoring dan Maintenance

### 1. Log Monitoring
```bash
# Apache logs
tail -f /var/log/apache2/access.log
tail -f /var/log/apache2/error.log

# MySQL logs
tail -f /var/log/mysql/error.log
```

### 2. Backup Database
```bash
# Backup manual
mysqldump -u root -p wordpress > wordpress_backup.sql

# Backup otomatis (cron)
0 0 * * * mysqldump -u root -p'password' wordpress > /backup/wordpress_$(date +\%Y\%m\%d).sql
```

### 3. File Permissions
```bash
# Set permissions yang aman
find /var/www/html/wordpress -type d -exec chmod 755 {} \;
find /var/www/html/wordpress -type f -exec chmod 644 {} \;
chmod 600 wp-config.php
```

## G. Troubleshooting Umum

### 1. Permission Issues
```bash
# Fix permission issues
sudo chown -R www-data:www-data /var/www/html/wordpress
sudo find /var/www/html/wordpress -type d -exec chmod 755 {} \;
sudo find /var/www/html/wordpress -type f -exec chmod 644 {} \;
```

### 2. Database Connection
```bash
# Test MySQL connection
mysql -u wpuser -p wordpress

# Check MySQL service
sudo systemctl status mysql
```

### 3. Apache/Nginx Issues
```bash
# Test Apache config
sudo apache2ctl configtest

# Test Nginx config
sudo nginx -t

# Restart services
sudo systemctl restart apache2
sudo systemctl restart nginx
```

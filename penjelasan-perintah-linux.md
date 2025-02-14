# Penjelasan Detail Perintah-Perintah Linux dan Cloud

## 1. Manajemen User dan Permission

### Perintah Pembuatan User
```bash
sudo useradd username
```
Perintah ini digunakan untuk membuat user baru di sistem Linux. Awalan `sudo` memberikan hak administratif untuk menjalankan perintah. Parameter `username` adalah nama user yang ingin dibuat. Setelah user dibuat, user tersebut belum memiliki password.

```bash
sudo passwd username
```
Perintah ini digunakan untuk membuat atau mengubah password untuk user yang sudah ada. Sistem akan meminta Anda memasukkan password baru dua kali untuk konfirmasi. Password harus memenuhi kriteria keamanan minimal sistem.

### Manajemen Group
```bash
sudo groupadd groupname
```
Perintah ini membuat group baru di sistem. Group digunakan untuk mengorganisir user dan mengatur permission secara kolektif. Satu user bisa menjadi anggota beberapa group.

```bash
sudo usermod -aG groupname username
```
Perintah ini menambahkan user ke dalam group. Flag `-a` berarti append (menambahkan) dan `-G` menspecifikasikan group. User tidak akan kehilangan keanggotaan di group lain yang sudah ada.

### Pengaturan Permission
```bash
chmod 755 file.txt
```
Perintah chmod mengubah permission file atau direktori. Angka 755 adalah notasi oktal yang berarti:
- 7 (owner): read (4) + write (2) + execute (1)
- 5 (group): read (4) + execute (1)
- 5 (others): read (4) + execute (1)

```bash
chown user:group file.txt
```
Perintah ini mengubah kepemilikan file. Format `user:group` menentukan user dan group yang akan menjadi pemilik file tersebut.

## 2. Package Management

### Update Sistem
```bash
sudo apt update
```
Perintah ini memperbarui daftar package yang tersedia dari repository. Ini tidak menginstall atau upgrade package, hanya memperbarui informasi tentang package yang tersedia.

```bash
sudo apt upgrade
```
Setelah update, perintah ini akan menginstall versi terbaru dari semua package yang sudah terinstall di sistem.

### Instalasi Package
```bash
sudo apt install package-name
```
Perintah untuk menginstall package baru. Apt akan secara otomatis menyelesaikan dependensi yang dibutuhkan package tersebut.

## 3. Web Server Setup

### Apache Installation
```bash
sudo apt install apache2
```
Menginstall web server Apache2. Apache adalah web server yang paling populer dan banyak digunakan di Linux.

```bash
sudo systemctl start apache2
```
Menjalankan service Apache2. Setelah dijalankan, web server akan mulai menerima request HTTP pada port 80.

```bash
sudo systemctl enable apache2
```
Mengatur Apache2 agar otomatis start ketika sistem boot. Ini penting untuk server production agar web server selalu running setelah restart.

### PHP Setup
```bash
sudo apt install php php-mysql php-curl php-gd php-mbstring php-xml php-xmlrpc
```
Menginstall PHP dan module-module yang sering dibutuhkan. Setiap module memberikan fungsi tambahan:
- php-mysql: untuk koneksi ke MySQL database
- php-curl: untuk melakukan HTTP request
- php-gd: untuk manipulasi gambar
- php-mbstring: untuk handling multibyte string
- php-xml dan php-xmlrpc: untuk processing XML

## 4. Docker Implementation

### Docker Installation
```bash
sudo apt install apt-transport-https ca-certificates curl software-properties-common
```
Menginstall package prerequisite yang dibutuhkan untuk menginstall Docker. Package ini memungkinkan apt menggunakan repository melalui HTTPS.

```bash
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
```
Menambahkan GPG key resmi Docker. Ini memverifikasi bahwa package yang didownload benar-benar dari Docker.

### Basic Docker Commands
```bash
docker pull nginx
```
Mengunduh image nginx dari Docker Hub. Image adalah template yang berisi aplikasi dan dependensinya.

```bash
docker run -d -p 80:80 nginx
```
Menjalankan container nginx:
- `-d`: run in detached mode (background)
- `-p 80:80`: map port 80 host ke port 80 container

## 5. AWS CLI Commands

### AWS Configuration
```bash
aws configure
```
Mengkonfigurasi AWS CLI dengan credentials dan preferensi default:
- AWS Access Key ID: kredensial untuk autentikasi
- AWS Secret Access Key: kredensial rahasia
- Default region: region AWS default
- Output format: format output (json/text/table)

### EC2 Management
```bash
aws ec2 run-instances --image-id ami-xxxxxxxx --instance-type t2.micro
```
Membuat instance EC2 baru dengan spesifikasi:
- `image-id`: ID dari Amazon Machine Image (AMI)
- `instance-type`: tipe instance yang menentukan CPU, memory, dan network performance

## 6. Security Implementation

### Firewall (UFW)
```bash
sudo ufw enable
```
Mengaktifkan firewall UFW (Uncomplicated Firewall). UFW adalah interface yang lebih user-friendly untuk iptables.

```bash
sudo ufw allow 80
```
Mengizinkan traffic pada port 80 (HTTP). Ini diperlukan untuk web server yang melayani request HTTP.

### SSL/TLS Setup
```bash
sudo certbot --apache -d example.com
```
Menginstall SSL certificate dari Let's Encrypt untuk domain menggunakan Certbot:
- `--apache`: gunakan plugin Apache
- `-d example.com`: specify domain name

## Tips Monitoring

### System Monitoring
```bash
top
```
Menampilkan proses yang sedang berjalan secara real-time, termasuk CPU usage, memory usage, dan informasi sistem lainnya.

```bash
df -h
```
Menampilkan penggunaan disk space dalam format human-readable (GB/MB).

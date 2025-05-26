# Ansible Lab: Automated Nginx Deployment & Testing in Docker

## 📋 Deskripsi Proyek

Proyek ini adalah lab DevOps berbasis Ansible untuk mengotomasi deployment, konfigurasi, dan pengujian Nginx pada beberapa node yang berjalan di dalam container Docker. Lab ini dirancang untuk pembelajaran dan simulasi environment produksi secara lokal, lengkap dengan playbook untuk instalasi, status check, dan testing Nginx.

## 🏗️ Struktur Proyek

```
ansible.cfg
docker-compose.yml
docker/
  Dockerfile
inventory/
  hosts.ini
  hosts.yml
playbooks/
  install-nginx.yml
  status-check.yml
  test-nginx.yml
```

- **ansible.cfg**: Konfigurasi utama Ansible.
- **docker/**: Dockerfile untuk membangun image node managed.
- **docker-compose.yml**: Orkestrasi container Docker untuk node managed.
- **inventory/**: File inventory dalam format YAML & INI.
- **playbooks/**: Kumpulan playbook Ansible untuk deployment, pengecekan status, dan pengujian Nginx.

## 🚀 Cara Menjalankan

### 1. Prasyarat

- [Docker](https://docs.docker.com/get-docker/)
- [Docker Compose](https://docs.docker.com/compose/install/)
- [Ansible](https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html)
- SSH key untuk user `ansible` (default: `~/.ssh/ansible-lab`)

### 2. Build & Jalankan Container

```sh
docker compose up --build -d
```

### 3. Setup SSH Key (Jika Belum Ada)

Generate SSH key (jika belum ada):

```sh
ssh-keygen -t rsa -b 4096 -f ~/.ssh/ansible-lab
```

Copy public key ke container :

```sh
# Copy key ke managed node 1
ssh-copy-id -i ~/.ssh/ansible-lab.pub -p 2201 ansible@localhost

# Copy key ke managed node 2  
ssh-copy-id -i ~/.ssh/ansible-lab.pub -p 2202 ansible@localhost

# Test SSH connection
ssh -i ~/.ssh/ansible-lab -p 2201 ansible@localhost "hostname && whoami"
ssh -i ~/.ssh/ansible-lab -p 2202 ansible@localhost "hostname && whoami"
```

### 4. Cek Koneksi Ansible

```sh
ansible all -m ping
```

### 5. Jalankan Playbook

#### a. Install & Konfigurasi Nginx

```sh
ansible-playbook playbooks/install-nginx.yml
```

#### b. Cek Status Sistem

```sh
ansible-playbook playbooks/status-check.yml
```

#### c. Test Nginx

```sh
ansible-playbook playbooks/test-nginx.yml
```

## 🌐 Akses Nginx

Setelah deployment, akses Nginx pada:

- Node 1: [http://localhost:8081](http://localhost:8081)
- Node 2: [http://localhost:8082](http://localhost:8082)

## 📂 Penjelasan File Penting

- **[playbooks/install-nginx.yml](playbooks/install-nginx.yml)**  
  Instalasi, konfigurasi, dan verifikasi Nginx secara otomatis.
- **[playbooks/status-check.yml](playbooks/status-check.yml)**  
  Mengecek uptime, disk, memory, dan status service Nginx.
- **[playbooks/test-nginx.yml](playbooks/test-nginx.yml)**  
  Pengujian port, HTTP response, konfigurasi, dan log Nginx.
- **[inventory/hosts.yml](inventory/hosts.yml)**  
  Inventory utama (format YAML) untuk Ansible.
- **[docker/Dockerfile](docker/Dockerfile)**  
  Image dasar Ubuntu + SSH + user ansible.
- **[docker-compose.yml](docker-compose.yml)**  
  Orkestrasi dua node managed dengan port forwarding.

## 🛠️ Customisasi

- **Tambah Node:**  
  Edit `docker-compose.yml` dan `inventory/hosts.yml` untuk menambah node baru.
- **Ubah Port:**  
  Sesuaikan mapping port di `docker-compose.yml` dan variabel di inventory.

## ⚠️ Catatan Keamanan

- Default password user `ansible` dan `root` adalah `ansible123` (hanya untuk lab/testing).
- Jangan gunakan konfigurasi ini di production tanpa modifikasi keamanan lebih lanjut.

## 📚 Referensi

- [Ansible Documentation](https://docs.ansible.com/)
- [Docker Documentation](https://docs.docker.com/)
- [Nginx Documentation](https://nginx.org/en/docs/)

## 👨‍💻 Kontributor

- Muhammad Dafa Ardiansyah

---

> Proyek ini dibuat untuk keperluan pembelajaran DevOps di DigitalSkola - Homework ke 7.
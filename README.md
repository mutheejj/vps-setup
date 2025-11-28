# VPS & Domain Setup Guide

This repository contains a complete set of guides that explain how to set up a VPS, connect a domain and subdomains, configure Nginx, enable SSL, and upload project files to your server. It is designed to be simple, beginner-friendly, and fully command-line oriented so you can follow each step without confusion.

---

## 📌 What This Repository Covers

The repository includes four main setup guides:

### **1. Server Setup** (`01-server-setup.md`)

Covers:

* How to connect your domain (Name.com → DigitalOcean)
* How to install Nginx on a fresh Ubuntu server
* How to enable SSL using Certbot
* How to access the default Nginx page

This gives you the foundation of a functional VPS accessible via your domain.

---

### **2. Domain & Subdomain Setup** (`02-domain-and-subdomain.md`)

Covers:

* How to register a domain on Name.com
* How to create subdomains
* How to configure A and CNAME records
* How DNS propagation works

This file is your DNS management guide.

---

### **3. File Upload Guide** (`03-upload-files-to-server.md`)

Explains multiple ways to upload files to your VPS, including:

* SCP (Linux/macOS)
* WinSCP (Windows)
* FileZilla (SFTP)
* Git (clone/pull from GitHub)
* Direct server download (wget/cURL)

This allows you to move your website/app files to the VPS.

---

### **4. Subdomain → VPS Mapping** (`04-subdomain-to-vps.md`)

Covers:

* How to point subdomains to your VPS
* How to host multiple sites on a single server
* How to bind each domain/subdomain to its own directory
* How to create separate Nginx configs for each site
* How to enable SSL for all domains/subdomains

This makes it possible to run many independent projects on one VPS.

---

## 🗂 Repository Structure

```
vps-setup/
│
├── 01-server-setup.md
├── 02-domain-and-subdomain.md
├── 03-upload-files-to-server.md
├── 04-subdomain-to-vps.md
└── README.md
```

---

## 🎯 Purpose of This Repository

This repository serves as a **personal reference manual** for:

* Managing DigitalOcean VPS instances
* Managing domains/subdomains on Name.com
* Deploying multiple sites on one server
* Uploading files and code to the VPS
* Setting up secure Nginx configurations with SSL

It can also be shared with team members, clients, or anyone learning server setup.

---

## 🚀 How to Use

1. Start with **01-server-setup.md** to configure your VPS.
2. Use **02-domain-and-subdomain.md** to prepare your DNS records.
3. Upload your files using one of the methods in **03-upload-files-to-server.md**.
4. Use **04-subdomain-to-vps.md** to host multiple sites on the same VPS.

Follow the files in order to avoid errors.

---

## 💡 Future Additions (Optional)

* Laravel deployment guide
* Node.js + PM2 deployment
* Dockerized app deployment
* Backup automation for VPS

If you'd like these added, they can be generated as Markdown files.

---

## 📞 Support

For any additions or improvements, feel free to update this repo or request more guides.


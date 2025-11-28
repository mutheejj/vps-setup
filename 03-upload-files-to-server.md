# How to Upload Files to a Linux VPS Server (Ubuntu)

This guide explains the simplest methods to upload project files to your VPS.

---

## 1. Upload Files Using SCP (Linux & macOS)

### **Upload a single file:**

```bash
scp localfile.zip username@server-ip:/var/www/project/
```

### **Upload a folder:**

```bash
scp -r myfolder/ username@server-ip:/var/www/project/
```

Replace:

* `localfile.zip` or `myfolder/` → your file/folder
* `username` → usually `root` unless changed
* `server-ip` → your VPS IP

---

## 2. Upload Files Using WinSCP (Windows)

1. Download WinSCP: [https://winscp.net](https://winscp.net)
2. Open WinSCP → **New Site**.
3. Select **SFTP**.
4. Enter:

   * Host: *server-ip*
   * Username: `root`
   * Password (or SSH key)
5. Click **Login**.
6. Drag & drop files into:

```
/var/www/project/
```

---

## 3. Upload Files Using FileZilla (Cross‑Platform)

1. Download FileZilla: [https://filezilla-project.org](https://filezilla-project.org)
2. Open FileZilla.
3. Go to **File → Site Manager**.
4. Create a new site with:

   * Protocol: **SFTP**
   * Host: server-ip
   * Username: root
   * Password or SSH key
5. Connect.
6. Upload files to:

```
/var/www/project/
```

---

## 4. Upload Using Git (Deploy from GitHub)

### **Clone your repo onto the server:**

```bash
sudo apt install git -y
git clone https://github.com/username/repo.git /var/www/project
```

### **Pull updates later:**

```bash
cd /var/www/project
git pull
```

---

## 5. Upload Using cURL or wget (Direct Download on Server)

### **Download a file directly to server:**

```bash
wget https://example.com/file.zip -P /var/www/project/
```

### **Or using curl:**

```bash
curl -o /var/www/project/file.zip https://example.com/file.zip
```

---

## 6. Extract and Set Permissions

### **Unzip:**

```bash
sudo apt install unzip -y
unzip file.zip -d /var/www/project/
```

### **Set file ownership for NGINX/Apache:**

```bash
sudo chown -R www-data:www-data /var/www/project/
```

---

## 7. Restart Server After Upload (if needed)

### **For NGINX:**

```bash
sudo systemctl restart nginx
```

### **For Apache:**

```bash
sudo systemctl restart apache2
```

---

## Done

Your files are now uploaded and served from your VPS.


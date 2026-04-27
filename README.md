# 🖥️ Linux & Web Server — আমার Personal Cheatsheet

> নিজের শেখার জন্য এবং ভবিষ্যতে দ্রুত রেফারেন্স করার জন্য তৈরি।

---

## 📁 ফাইল ও ফোল্ডার ম্যানেজমেন্ট

```bash
# ফোল্ডারের সব ফাইল মুছে ফেলা (লুকানো ফাইলসহ)
rm -rf *
rm -rf .[^.]*

# ফোল্ডার delete করা
sudo rm -rf /opt/vscode

# ফাইল দেখা / এডিট করা
cat filename
nano filename
```

---

## 🔐 Permission & Ownership

```bash
# Apache কে ownership দেওয়া
sudo chown -R apache:apache /var/www/html

# নিজেকে htdocs-এর মালিক বানানো (LAMPP-এর জন্য)
sudo chown -R $USER:$USER /opt/lampp/htdocs/

# Permission সেট করা
sudo chmod -R 775 storage bootstrap/cache
sudo chmod -R 777 /var/www/html/custom
sudo chmod 755 filename
sudo chmod +x filename   # executable করা

# কোনো command কোথায় আছে জানা
which code
```

---

## 🌐 Git & GitHub

```bash
# Repo clone করে বর্তমান ফোল্ডারে রাখা
git clone https://github.com/SohanTgs/NamsWeb-Main.git .
```

> ⚠️ **সতর্কতা:** GitHub Personal Access Token (PAT) কখনো publicly শেয়ার করবে না।  
> Token expose হলে সাথে সাথে GitHub Settings → Developer Settings → Tokens থেকে revoke করো।

---

## 🐘 PHP Version ম্যানেজমেন্ট

```bash
# নতুন PHP version install করা
sudo apt update
sudo apt install software-properties-common -y
sudo add-apt-repository ppa:ondrej/php
sudo apt update

# PHP version switch করা
sudo update-alternatives --config php
```

---

## 🗄️ MySQL / Database

```bash
# নতুন User তৈরি ও সব permission দেওয়া
CREATE USER 'admin'@'localhost' IDENTIFIED BY '123456';
GRANT ALL PRIVILEGES ON *.* TO 'admin'@'localhost' WITH GRANT OPTION;
FLUSH PRIVILEGES;

# Root user-এর authentication method পরিবর্তন
ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'root';
FLUSH PRIVILEGES;
EXIT;

# .sql ফাইল থেকে database import করা
mysql -u root -p database_name < /var/www/html/project/db/database.sql
mysql -u root -p database_name < /home/users/downloads/database.sql
```

---

## 🔗 Apache Web Server

```bash
# Apache বন্ধ ও disable করা
sudo systemctl stop apache2
sudo systemctl disable apache2

# phpMyAdmin manually link করা
sudo ln -s /usr/share/phpmyadmin /var/www/html/phpmyadmin

# Apache config এডিট করা
sudo nano /etc/apache2/apache2.conf
```

**`apache2.conf`-এ গুরুত্বপূর্ণ পরিবর্তন:**

```apache
<Directory /var/www/>
    AllowOverride All    # ← "None" থেকে "All" করতে হবে (`.htaccess` কাজ করানোর জন্য)
</Directory>
```

---

## 🔑 SSH Key Setup

```bash
# নিজের PC-তে SSH key তৈরি করা
ssh-keygen

# Public key দেখা (server-এ যোগ করার জন্য)
cat ~/.ssh/id_rsa.pub
```

> তারপর এই public key টি server-এর `~/.ssh/authorized_keys` ফাইলে যোগ করতে হবে।

---

## 🧹 System Cleanup

```bash
# Software uninstall করা
sudo apt purge composer

# অপ্রয়োজনীয় package সরানো ও cache পরিষ্কার
sudo apt autoremove
sudo apt clean
```

---

## 📦 Installed Package দেখা

```bash
# সব installed package দেখা
sudo apt list --installed

# PHP সম্পর্কিত package ফিল্টার করা
sudo apt list --installed | grep php

# Manually install করা package দেখা
apt-mark showmanual
```

---

## 🖥️ GUI / Desktop Shortcuts

```bash
# File manager-এ বর্তমান ফোল্ডার খোলা
sudo xdg-open .

# Browser-এ localhost খোলা
sudo xdg-open http://localhost
```

---

## 📋 GitHub Actions / CI-CD Secret Variables

| Variable | কাজ |
|---|---|
| `SSH_HOST` | Server-এর IP address |
| `SSH_USER` | Server-এর SSH username |
| `SSH_KEY` | Server-এর private SSH key |
| `GHP_TOKEN` | GitHub Personal Access Token |

> এগুলো **GitHub → Repository → Settings → Secrets and variables → Actions**-এ যোগ করতে হয়।

---

## ⚡ Quick Reference (সবচেয়ে বেশি ব্যবহৃত)

| কাজ | Command |
|---|---|
| ফাইল দেখা | `cat file` বা `nano file` |
| Permission দেওয়া | `sudo chmod -R 775 folder/` |
| Ownership দেওয়া | `sudo chown -R user:group folder/` |
| Git clone | `git clone <url> .` |
| PHP switch | `sudo update-alternatives --config php` |
| MySQL ঢোকা | `mysql -u root -p` |
| Package install | `sudo apt install package-name` |
| Package remove | `sudo apt purge package-name` |
| System clean | `sudo apt autoremove && sudo apt clean` |

---

*Last updated: April 2026*

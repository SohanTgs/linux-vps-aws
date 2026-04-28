# 🖥️ Linux / VPS / AWS — Cheatsheet

> Created for learning and quick future reference.

---

## 📁 File & Folder Management

```bash
# Delete all files in a folder (including hidden files)
rm -rf *
rm -rf .[^.]*

# Delete a folder
sudo rm -rf /opt/vscode

# Delete all files inside /var/www/html (including hidden files)
sudo rm -rf /var/www/html/*
sudo rm -rf /var/www/html/.[!.]*
sudo rm -rf /var/www/html/..?*

# View / edit a file
cat filename
nano filename
```

---

## 🔐 Permission & Ownership

```bash
# Give ownership to Apache
sudo chown -R apache:apache /var/www/html

# Give ownership to current user
sudo chown -R $USER:$USER /var/www/html

# Give ownership to ubuntu user
sudo chown ubuntu:ubuntu index.html          # single file
sudo chown -R ubuntu:ubuntu /var/www/        # entire project folder (Laravel best practice)

# Give ownership to www-data (Apache web server user)
sudo chown -R www-data:www-data /var/www/html

# Take ownership of htdocs (for LAMPP)
sudo chown -R $USER:$USER /opt/lampp/htdocs/

# Set permissions
sudo chmod -R 775 storage bootstrap/cache
sudo chmod -R 775 /var/www/html/storage
sudo chmod -R 775 /var/www/html/bootstrap/cache
sudo chmod -R 777 /var/www/html/custom
sudo chmod 755 filename
sudo chmod +x filename   # make executable

# Find where a command is installed
which code
```

> **What permission numbers mean:**
> - `7` = read + write + execute (rwx)
> - `5` = read + execute (r-x)
> - `775` = full access for owner & group, others can only read/execute
> - `777` = everyone has full access (use in development only, never in production)

> **Ownership tips (VPS/AWS):**
> - `ubuntu:ubuntu` → Ubuntu server এ deploy করার সময় (SSH user)
> - `www-data:www-data` → Apache যখন files serve করে তখন
> - `$USER:$USER` → Local development এ নিজে access নিতে

---

## 🌐 Git & GitHub

```bash
# Clone a repo into the current folder
git clone https://github.com/username/repo-name.git .
```

> ⚠️ **Warning:** Never share your GitHub Personal Access Token (PAT) publicly.
> If a token is exposed, immediately revoke it from **GitHub → Settings → Developer Settings → Tokens**.

---

## 🐘 PHP Version Management

```bash
# Install a new PHP version
sudo apt update
sudo apt install software-properties-common -y
sudo add-apt-repository ppa:ondrej/php
sudo apt update

# Switch PHP version
sudo update-alternatives --config php

# Install PHP extensions
sudo apt update && sudo apt install php-bcmath
sudo apt install php8.3-bcmath      # version-specific install
```

---

## 🗄️ MySQL / Database

```bash
# Enter MySQL
mysql -u root -p

# Create a new user and grant all privileges
CREATE USER 'admin'@'localhost' IDENTIFIED BY '123456';
GRANT ALL PRIVILEGES ON *.* TO 'admin'@'localhost' WITH GRANT OPTION;
FLUSH PRIVILEGES;

# Change root user's authentication method
ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'root';
FLUSH PRIVILEGES;
EXIT;

# Import a database from a .sql file
mysql -u root -p database_name < /path/to/database.sql
```

---

## 🔗 Apache Web Server

```bash
# Start / stop / restart Apache
sudo systemctl start apache2
sudo systemctl stop apache2
sudo systemctl restart apache2
sudo systemctl disable apache2

# Enable mod_rewrite (required for Laravel / .htaccess)
sudo a2enmod rewrite
sudo systemctl restart apache2

# Check Apache virtual host configuration
apache2ctl -S

# Manually link phpMyAdmin
sudo ln -s /usr/share/phpmyadmin /var/www/html/phpmyadmin

# Edit Apache config
sudo nano /etc/apache2/apache2.conf
```

**Important change in `apache2.conf`:**

```apache
<Directory /var/www/>
    AllowOverride All    # ← Change "None" to "All" (required for .htaccess to work)
</Directory>
```

---

## 🔑 SSH Key Setup

```bash
# Generate an SSH key on your local PC
ssh-keygen

# View the public key (to add to the server)
cat ~/.ssh/id_rsa.pub

# Connect to server using SSH key (AWS / EC2)
ssh -i your-key.pem ubuntu@13.203.207.80

# Connect to server using password (root login)
ssh root@ip
```

> Then add the public key to the server's `~/.ssh/authorized_keys` file.

> **AWS tip:** `.pem` file এর permission ঠিক না থাকলে error দেবে। Fix:
> ```bash
> chmod 400 your-key.pem
> ```

---

## 🧹 System Cleanup

```bash
# Uninstall a software package
sudo apt purge package-name

# Remove unused packages and clean cache
sudo apt autoremove
sudo apt autoclean
sudo apt clean
```

---

## 📦 Viewing Installed Packages

```bash
# List all installed packages
sudo apt list --installed

# Filter for PHP-related packages
sudo apt list --installed | grep php

# List manually installed packages
apt-mark showmanual
```

---

## 🖥️ GUI / Desktop Shortcuts

```bash
# Open current folder in file manager
sudo xdg-open .

# Open localhost in browser
sudo xdg-open http://localhost
```

---

## 📋 GitHub Actions / CI-CD Secret Variables

| Variable | Purpose |
|---|---|
| `SSH_HOST` | Server's IP address |
| `SSH_USER` | Server's SSH username |
| `SSH_KEY` | Server's private SSH key |
| `GHP_TOKEN` | GitHub Personal Access Token |

> Add these under **GitHub → Repository → Settings → Secrets and variables → Actions**.

---

## ⚡ Quick Reference (Most Used Commands)

| Task | Command |
|---|---|
| View a file | `cat file` or `nano file` |
| Set permissions | `sudo chmod -R 775 folder/` |
| Set ownership | `sudo chown -R user:group folder/` |
| Git clone | `git clone <url> .` |
| Switch PHP | `sudo update-alternatives --config php` |
| Enter MySQL | `mysql -u root -p` |
| Install a package | `sudo apt install package-name` |
| Remove a package | `sudo apt purge package-name` |
| Clean system | `sudo apt autoremove && sudo apt clean` |
| SSH connect (key) | `ssh -i key.pem ubuntu@ip` |
| SSH connect (root) | `ssh root@ip` |
| Enable mod_rewrite | `sudo a2enmod rewrite` |
| Check Apache vhosts | `apache2ctl -S` |

---

*Last updated: April 2026*

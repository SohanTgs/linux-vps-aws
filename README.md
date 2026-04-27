# 🖥️ সার্ভার, Git ও PHP কমান্ডসহ চিটশিট

এই ডকুমেন্টটি দৈনন্দিন কাজে ব্যবহৃত গুরুত্বপূর্ণ কমান্ডের একটি সংগ্রহ। বিষয়ভিত্তিক ভাগ করে দেওয়া হয়েছে দ্রুত খুঁজে পাওয়ার সুবিধার্থে।

---

## 🚀 প্রাথমিক ওয়েব সার্ভার সেটআপ

```bash
# ওয়েব রুটে গিয়ে সব ফাইল মুছে ফেলা (সাবধানে ব্যবহার করুন!)
cd /var/www/html
rm -rf *
rm -rf .[^.]*   # লুকানো ফাইলও মুছে যাবে

# গিট থেকে ক্লোন করা
git clone https://github.com/SohanTgs/NamsWeb-Main.git .

# পারমিশন সার্ভিস সেটআপ
sudo chown -R apache:apache /var/www/html
sudo chmod -R 775 storage bootstrap/cache


# আপনার লোকাল পিসিতে SSH কী তৈরি
ssh-keygen

# পাবলিক কী দেখুন (সার্ভারে যোগ করতে)
cat ~/.ssh/id_rsa.pub

# গিট হাব টোকেন (গোপন রাখুন)
# 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s (নমুনা টোকেন)

# সার্ভারের SSH তথ্য (ডটসহ পরিবর্তন করবেন)
SSH_HOST=your_server_ip
SSH_USER=your_username
SSH_KEY=~/.ssh/id_rsa


CREATE USER 'admin'@'localhost' IDENTIFIED BY '123456';
GRANT ALL PRIVILEGES ON *.* TO 'admin'@'localhost' WITH GRANT OPTION;
FLUSH PRIVILEGES;


# এসকিউএল ফাইল ইমপোর্ট
mysql -u root -p database_name < /path/to/file.sql


# নতুন PHP সংস্করণ যোগ করা
sudo apt update
sudo apt install software-properties-common -y
sudo add-apt-repository ppa:ondrej/php
sudo apt update

# PHP ভার্সন সুইচ করা
sudo update-alternatives --config php

# নির্দিষ্ট প্যাকেজ আনইন্সটল
sudo apt purge composer

# অপ্রয়োজনীয় ফাইল ও ডিপেন্ডেন্সি清除
sudo apt autoremove
sudo apt clean


# এক্সিকিউটেবল করার অনুমতি দেওয়া
sudo chmod 755 filename
sudo chmod +x filename

# সম্পূর্ণ ডিরেক্টরির অনুমতি পরিবর্তন
sudo chmod -R 777 /var/www/html/custom

# মালিকানা পরিবর্তন (ল্যাম্পের জন্য htdocs)
sudo chown -R $USER:$USER /opt/lampp/htdocs/

# অ্যাপাচি কনফিগ ফাইলে AllowOverride পরিবর্তন
sudo nano /etc/apache2/apache2.conf
# ভিতরে <Directory /var/www/> ব্লকে AllowOverride None কে All করুন


# অ্যাপাচি বন্ধ ও ডিজেবল
sudo systemctl stop apache2
sudo systemctl disable apache2

# ইনস্টল করা প্যাকেজের তালিকা
sudo apt list --installed
sudo apt list --installed | grep php

# প্রোগ্রামের লোকেশন জানতে
which code


# ফাইল ব্রাউজারে বর্তমান ডিরেক্টরি খোলা
sudo xdg-open .

# লোকালহোস্ট ব্রাউজারে খোলা
sudo xdg-open http://localhost

# ফাইল দেখা
nano filename
cat filename

# phpMyAdmin লিংক আপ করা
sudo ln -s /usr/share/phpmyadmin /var/www/html/phpmyadmin

# রুট পাসওয়ার্ড পরিবর্তন (MySQL/MariaDB)
ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'root';
FLUSH PRIVILEGES;
EXIT;

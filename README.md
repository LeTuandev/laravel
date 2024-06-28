### vòng đời của một request trong laravel
chạy vào file public/index.php => bootstrap/app.php => htt/kernel.php => service provider => router => middleware => controller => view

### Các bước đơn giản để cài lemp stack(linux, nginx, mysql, php)
Step1: install nginx server
  - sudo apt update
  - sudo apt install nginx
* Cấu hình hạn chế lưu lượng truy cập cho nginx
  - sudo ufw allow 'Nginx HTTP'
Step2: Install mysql
  - sudo apt install mysql-server
  - sudo mysql_secure_installation
* Note: nếu password đang là mặc định của mysql server thì thực hiện change password
  https://www.digitalocean.com/community/tutorials/how-to-reset-your-mysql-or-mariadb-root-password-on-ubuntu-20-04
  - chạy lệnh mysql -u root -p để test kết nối và tạo database(create database example;)
Step3: Install php
  - sudo apt install php-fpm php-mysql
Step4: config nginx to use php process
  - sudo mkdir /var/www/example
  - cd /var/www/example -> git clone link_project
  - Change permission cho /var/www/example (sudo chown -R $USER:$USER /var/www/example, then: 'sudo find . -type f -exec chmod 664 {} \;' , after: 'sudo find . -type d -exec chmod 775 {} \;', continue: 'sudo chgrp -R www-data storage bootstrap/cache', and 'sudo chmod -R ug+rwx storage bootstrap/cache')
  - sudo nano /etc/nginx/sites-available/example và paste
      server {
      listen 80;
      server_name your_domain www.your_domain;
      root /var/www/your_domain;
  
      index index.html index.htm index.php;
  
      location / {
          try_files $uri $uri/ =404;
      }
  
      location ~ \.php$ {
          include snippets/fastcgi-php.conf;
          fastcgi_pass unix:/var/run/php/php7.4-fpm.sock;
       }
  
      location ~ /\.ht {
          deny all;
      }
  - sudo ln -s /etc/nginx/sites-available/ /etc/nginx/sites-enabled/
  - sudo unlink /etc/nginx/sites-enabled/default
  - sudo nginx -t
  - sudo systemctl reload nginx or sudo systemctl restart 
}

### just user data, use with Amazon Linux 2. Rest literature is in progress ;)

`#!/bin/bash
yum install httpd mysql git -y
amazon-linux-extras install -y php7.2
cd /var/www/html
git clone https://github.com/ggnsng/bikerhood.git
mv /var/www/html/bikerhood/*  /var/www/html/
service httpd start
chkconfig httpd on`

`#!/bin/bash
yum install httpd mysql git -y
amazon-linux-extras install -y php7.2
cd /var/www/html
git clone https://github.com/ashydv/bikerhood.git
mv /var/www/html/bikerhood/*  /var/www/html/
service httpd start
chkconfig httpd on`

`#!/bin/bash  
yum install httpd mysql git -y`

`#!/bin/bash`
`yum install httpd mysql git -y`

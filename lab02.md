### use the below user data for autoscalling and loadbalancing lab.

    #!/bin/bash
    yum install httpd git -y
    amazon-linux-extras install -y php7.2
    cd /var/www/html
    git clone https://github.com/ggnsng/luckybuy.git
    mv /var/www/html/luckybuy/*  /var/www/html/
    service httpd start
    chkconfig httpd on

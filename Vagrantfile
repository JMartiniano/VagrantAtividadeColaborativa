Vagrant.configure("2") do |config|
  config.vm.box = "ubuntu/xenial64"
  config.vm.network "public_network"
  config.vm.provision "shell", inline: <<-SHELL
    #Atualizando o sistema
    apt-get update
    apt upgrade
    #Instalando o NGINX e ferramentas necessárias
    apt-get install -y vim
    apt-get install -y nginx
    #Instlaando o MySQL
    sudo apt-get -y install debconf-utils
    echo "mysql-server-5.1 mysql-server/root_password password teste" > ~/mysql.preseed
    echo "mysql-server-5.1 mysql-server/root_password_again password teste" >> ~/mysql.preseed
    echo "mysql-server-5.1 mysql-server/start_on_boot boolean true" >> ~/mysql.preseed
    cat ~/mysql.preseed | sudo debconf-set-selections
    sudo apt-get -y install mysql-server
    #Configurando o MySQL
    mysql -u root -p
    mysql> CREATE DATABASE WordPress CHARACTER SET utf8mb4 COLLATE utf8mb4_general_ci;
    mysql> GRANT ALL ON WordPress.* TO WordPressUser @'localhost' IDENTIFIED BY 'your password';
    mysql> FLUSH PRIVILEGES;
    mysql> EXIT;
    #Instalando php
    sudo apt-get -y install php5
    sudo apt-get -y install php5-cli
    sudo apt-get -y install php5-memcached
    sudo apt-get -y install php5-gd
    sudo apt-get -y install php5-xdebug
    sudo apt-get -y install php5-mcrypt
    sudo apt-get -y install php5-mysql
    #Instalando e configurando WordPress
    apt-get install -y curl
    mkdir -p /var/www/html/sample.com
    touch script.sh
    curl -O https://wordpress.org/latest.tar.gz
    tar xzvf latest.tar.gz
    echo "#!/bin/bash" >> script.sh
    echo "mv ./wordpress/* /var/www/html/sample.com/" >> script.sh
    echo "chown -R www-data: /var/www/html/sample.com" >> script.sh
    echo "ln -s /etc/nginx/sites-available/sample.com /etc/nginx/sites-enabled/" >> script.sh
    chmod +x script.sh
    ./script.sh
    nginx -t
    systemctl restart nginx
  SHELL
end

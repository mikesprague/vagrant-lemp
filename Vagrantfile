# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
  config.vm.box = "ubuntu/xenial64"
  config.vm.boot_timeout = 600
  config.vm.network "private_network", ip: "192.168.50.75", auto_correct: true
  config.vm.synced_folder "webroot", "/usr/share/nginx/html", owner: "www-data", group: "www-data"
  config.vm.provider "virtualbox" do |v|
    v.name = "Vagrant-LEMP"
    v.customize ["modifyvm", :id, "--cpuexecutioncap", "80"]
    v.customize ["modifyvm", :id, "--memory", 2048]
    v.customize ["modifyvm", :id, "--cpus", 1]
  end

  if defined?(VagrantPlugins::HostsUpdater)
    config.vm.hostname = "lemp.dev"
    config.hostsupdater.aliases = [
      "www.lemp.dev"
    ]
  end

  config.vm.provision "shell", inline:<<-SHELL
    echo ""
    echo ""
    echo "======================================="
    echo "|   Initializing Vagrant LEMP Setup   |"
    echo "======================================="
    echo ""

    echo ""
    echo "Updating Linux ..."
    sudo apt-get -qq update >/dev/null 2>&1
    sudo apt-get -y upgrade >/dev/null 2>&1
    echo "... done updating Linux."


    echo ""
    echo "Installing common packages ..."
    sudo apt-get -y install vim curl build-essential python-software-properties git >/dev/null 2>&1
    sudo apt-get -y -f install >/dev/null 2>&1
    echo "... done installing common packages."

    echo ""
    echo "Installing Nginx ..."
    sudo apt-add-repository -y ppa:nginx/development >/dev/null 2>&1
    sudo apt-get -y update >/dev/null 2>&1
    sudo rm -f /usr/share/nginx/html/index.html
    sudo apt-get -y install nginx >/dev/null 2>&1
    sudo apt-get -y -f install >/dev/null 2>&1
    sudo chown www-data /usr/share/nginx/html -R >/dev/null 2>&1
    echo "... configuring Nginx ..."
    sudo systemctl enable nginx >/dev/null 2>&1
    sudo systemctl start nginx >/dev/null 2>&1
    # sudo systemctl status nginx
    echo "... done installing Nginx."

    echo ""
    echo "Installing MariaDB ..."
    sudo debconf-set-selections <<< "maria-db-server-10.1 mysql-server/root_password password root" >/dev/null 2>&1
    sudo debconf-set-selections <<< "maria-db-server-10.1 mysql-server/root_password_again password root" >/dev/null 2>&1
    sudo apt-get -y install mariadb-server >/dev/null 2>&1
    sudo apt-get -y -f install >/dev/null 2>&1
    echo "... configuring and securing MariaDB ..."
    sudo systemctl enable mysql >/dev/null 2>&1
    sudo systemctl start mysql >/dev/null 2>&1
    echo "DELETE FROM mysql.user WHERE User='';" | mysql -uroot -proot
    echo "DELETE FROM mysql.user WHERE User='root' AND Host NOT IN ('localhost', '127.0.0.1', '::1');" | mysql -uroot -proot
    echo "DROP DATABASE IF EXISTS test;" | mysql -uroot -proot
    echo "DELETE FROM mysql.db WHERE Db='test' OR Db='test\\_%';" | mysql -uroot -proot
    echo "FLUSH PRIVILEGES;" | mysql -uroot -proot
    sudo systemctl reload mysql >/dev/null 2>&1
    # sudo systemctl status mysql
    echo "... done installing MariaDB."

    echo ""
    echo "Installing PHP 7 ..."
    sudo apt-get -y install php7.0-fpm php7.0-mbstring php7.0-xml php7.0-xmlrpc php7.0-mysql php7.0-common php7.0-gd php7.0-json php7.0-cli php7.0-curl php-mcrypt php-memcached >/dev/null 2>&1
    sudo apt-get -y -f install >/dev/null 2>&1
    sudo systemctl start php7.0-fpm >/dev/null 2>&1
    # sudo systemctl status php7.0-fpm
    echo "... configuring PHP 7 for Nginx ..."
    sudo cp /vagrant/config/nginx-default.conf /etc/nginx/conf.d/default.conf >/dev/null 2>&1
    sudo nginx -t >/dev/null 2>&1
    sudo systemctl reload nginx >/dev/null 2>&1
    echo "... done installing PHP 7."

    echo ""
    echo "Installing Composer ..."
    sudo curl -sS https://getcomposer.org/installer | php >/dev/null 2>&1
    sudo mv composer.phar /usr/local/bin/composer >/dev/null 2>&1
    echo "... done installing Composer."

    echo ""
    echo "Installing PHPUnit ..."
    sudo wget https://phar.phpunit.de/phpunit.phar >/dev/null 2>&1
    sudo chmod +x phpunit.phar >/dev/null 2>&1
    sudo mv phpunit.phar /usr/local/bin/phpunit >/dev/null 2>&1
    echo "... done installing PHPUnit."

    echo ""
    echo "Installing phpMyAdmin ..."
    sudo debconf-set-selections <<< "maria-db-server-10.1 mysql-server/root_password password root" >/dev/null 2>&1
    sudo debconf-set-selections <<< "maria-db-server-10.1 mysql-server/root_password_again password root" >/dev/null 2>&1
    sudo debconf-set-selections <<< "phpmyadmin phpmyadmin/dbconfig-install boolean true" >/dev/null 2>&1
    sudo debconf-set-selections <<< "phpmyadmin phpmyadmin/app-password-confirm password root" >/dev/null 2>&1
    sudo debconf-set-selections <<< "phpmyadmin phpmyadmin/mysql/admin-pass password root" >/dev/null 2>&1
    sudo debconf-set-selections <<< "phpmyadmin phpmyadmin/mysql/app-pass password root" >/dev/null 2>&1
    sudo debconf-set-selections <<< "phpmyadmin phpmyadmin/reconfigure-webserver multiselect none" >/dev/null 2>&1
    sudo apt-get -qq install phpmyadmin >/dev/null 2>&1
    sudo apt-get -y -f install >/dev/null 2>&1
    echo "... configuring phpMyAdmin ..."
    sudo ln -sf /usr/share/phpmyadmin /usr/share/nginx/html/phpmyadmin >/dev/null 2>&1
    echo "GRANT ALL PRIVILEGES ON *.* TO 'phpmyadmin'@'localhost' WITH GRANT OPTION;" | mysql -uroot -proot
    echo "FLUSH PRIVILEGES;" | mysql -uroot -proot
    sudo systemctl reload mysql >/dev/null 2>&1
    echo "... done installing phpMyAdmin."

    echo ""
    echo "======================================="
    echo "|     Vagrant LEMP Setup Complete     |"
    echo "======================================="
    echo ""
    echo "http://lemp.dev (192.168.50.75)"
    echo ""
    echo "phpMyAdmin"
    echo "http://lemp.dev/phpmyadmin"
    echo "User: phpmyadmin"
    echo "Pass: root"
    echo ""
    echo "======================================="
    echo ""
  SHELL
end

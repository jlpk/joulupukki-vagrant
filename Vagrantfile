# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure(2) do |config|

    config.vm.box = "ubuntu/trusty64"
    config.vm.network "forwarded_port", guest: 80, host: 8888

    config.vm.provider "virtualbox" do |vb|
    vb.memory = "1024"
    config.vm.synced_folder "source", "/home/vagrant/source"
    #config.vm.provision "file",   source: "apache.conf", destination: "/etc/apache2/sites-enabled/joulupukki.conf"
end


config.vm.provision "shell", privileged: false, inline: <<-SHELL
    # Install deps
    sudo apt-get update
    sudo apt-get install -y python-pip python-virtualenv git npm nodejs-legacy ruby ruby-sass docker.io mongodb rabbitmq-server apache2 build-essential python-dev tmux

    # Add apache conf

    # Start services
    sudo a2enmod proxy proxy_http
    sudo cp /vagrant/apache.conf /etc/apache2/sites-enabled/joulupukki.conf
    sudo service apache2 restart
    sudo service mongodb restart
    sudo service rabbitmq-server restart

    # Prepare python env
    cd /home/vagrant/source
    virtualenv /home/vagrant/env
    /home/vagrant/env/bin/pip install watchdog

    # Install of joulupukki-common
    cd /home/vagrant/source
    git clone https://github.com/jlpk/joulupukki-common common || echo "jlpk-common seems already here. Don't forget `git pull`"
    cd common
    /home/vagrant/env/bin/pip install -r requirements.txt
    /home/vagrant/env/bin/python setup.py develop

    # Install of joulupukki-worker
    cd /home/vagrant/source
    git clone https://github.com/jlpk/joulupukki-worker worker || echo "jlpk-worker seems already here. Don't forget `git pull`"
    cd worker
    /home/vagrant/env/bin/pip install -r requirements.txt
    /home/vagrant/env/bin/python setup.py develop
    #screen -S worker -d -m 
    #screen -S worker

    # Install of joulupukki-api
    cd /home/vagrant/source
    git clone https://github.com/jlpk/joulupukki-api api || echo "jlpk-api seems already here. Don't forget `git pull`"
    cd api
    /home/vagrant/env/bin/pip install -r requirements.txt
    /home/vagrant/env/bin/python setup.py develop

    # Install of joulupukki-dispatcher
    cd /home/vagrant/source
    git clone https://github.com/jlpk/joulupukki-dispatcher dispatcher || echo "jlpk-dispatcher seems already here. Don't forget `git pull`"
    cd dispatcher
    /home/vagrant/env/bin/pip install -r requirements.txt
    /home/vagrant/env/bin/python setup.py develop

    # Install of joulupukki-web
    cd /home/vagrant/source
    git clone https://github.com/jlpk/joulupukki-web web || echo "jlpk-web seems already here. Don't forget `git pull`"
    source /home/vagrant/source/web/dev_virtualenv
    #cd /home/vagrant/web && npm install grunt-cli
    #cd /home/vagrant/web && npm install

    #cd /home/vagrant/web && node_modules/grunt-cli/bin/grunt  

    SHELL
end

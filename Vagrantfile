# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure(2) do |config|

    config.vm.box = "ubuntu/trusty64"
    config.vm.network "forwarded_port", guest: 80, host: 8888

    config.vm.provider "virtualbox" do |vb|
    vb.memory = "1024"
    config.vm.synced_folder "source", "/home/vagrant/source"
end


config.vm.provision "shell", privileged: false, inline: <<-SHELL
    # Install deps
    sudo apt-get update
    sudo apt-get install -y python-pip python-virtualenv git npm nodejs-legacy ruby ruby-sass docker.io mongodb rabbitmq-server apache2 build-essential python-dev tmux

    # copy configuration tmux for scroll 
    cp /vagrant/tmux.conf /home/vagrant/.tmux.conf

    # Prepare python env
    cd /home/vagrant/source
    virtualenv /home/vagrant/env
    /home/vagrant/env/bin/pip install watchdog ipdb ipython

    # Install of joulupukki-web
    cd /home/vagrant/source
    git clone https://github.com/jlpk/joulupukki-web web || echo "jlpk-web seems already here. Don't forget 'git pull'"
    cd /home/vagrant/source/web
    npm install grunt-cli || true
    alias bower='/home/vagrant/source/web/node_modules/bower/bin/bower' && npm install 2>/dev/null || true
#    npm install 2>/dev/null || true
#    node_modules/bower/bin/bower install 2> /dev/null || true
    node_modules/grunt-cli/bin/grunt sass:dev

    # Install of joulupukki-common
    cd /home/vagrant/source
    git clone https://github.com/jlpk/joulupukki-common common || echo "jlpk-common seems already here. Don't forget 'git pull'"
    cd common
    /home/vagrant/env/bin/pip install -r requirements.txt
    /home/vagrant/env/bin/python setup.py develop

    # Install of joulupukki-worker
    cd /home/vagrant/source
    git clone https://github.com/jlpk/joulupukki-worker worker || echo "jlpk-worker seems already here. Don't forget 'git pull'"
    cd worker
    /home/vagrant/env/bin/pip install -r requirements.txt
    /home/vagrant/env/bin/python setup.py develop

    # Install of joulupukki-api
    cd /home/vagrant/source
    git clone https://github.com/jlpk/joulupukki-api api || echo "jlpk-api seems already here. Don't forget 'git pull'"
    cd api
    /home/vagrant/env/bin/pip install -r requirements.txt
    /home/vagrant/env/bin/python setup.py develop


    # Install of joulupukki-dispatcher
    cd /home/vagrant/source
    git clone https://github.com/jlpk/joulupukki-dispatcher dispatcher || echo "jlpk-dispatcher seems already here. Don't forget 'git pull'"
    cd dispatcher
    /home/vagrant/env/bin/pip install -r requirements.txt
    /home/vagrant/env/bin/python setup.py develop

    # Start services
    sudo a2dissite 000-default
    sudo usermod -a -G vagrant www-data
    sudo a2enmod proxy proxy_http
    sudo cp /vagrant/apache.conf /etc/apache2/sites-enabled/joulupukki.conf
    sudo service apache2 restart
    sudo service mongodb restart
    sudo service rabbitmq-server restart

    # Starting daemons
    echo "Starting Worker"
    tmux new -d -s worker
    tmux send -t worker 'cd /home/vagrant/source/worker' ENTER
    tmux send -t worker '/home/vagrant/env/bin/pecan serve --reload config.py' ENTER
    echo "Worker started"
    echo "Starting API"
    tmux new -d -s api
    tmux send -t api 'cd /home/vagrant/source/api' ENTER
    tmux send -t api '/home/vagrant/env/bin/pecan serve --reload config.py' ENTER
    echo "API started"
    echo "Starting Dispatcher"
    tmux new -d -s dispatcher
    tmux send -t dispatcher 'cd /home/vagrant/source/dispatcher' ENTER
    tmux send -t dispatcher '/home/vagrant/env/bin/pecan serve --reload config.py' ENTER
    echo "Dispatcher started"
    echo "Starting Grunt"
    tmux new -d -s web
    tmux send -t web 'cd /home/vagrant/source/web' ENTER
    tmux send -t web 'node_modules/grunt-cli/bin/grunt' ENTER
    echo "Grunt started"

    SHELL
end

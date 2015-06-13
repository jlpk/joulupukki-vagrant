# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure(2) do |config|

      	config.vm.box = "ubuntu/trusty64"

   	#config.vm.network "public_network"
	#config.vm.forward_port 8000, 8000
	config.vm.network "forwarded_port", guest: 8000, host: 8000
  
  config.vm.provision "shell", privileged: false, inline: <<-SHELL
     sudo apt-get update
     sudo apt-get install -y python-pip python-virtualenv git wget npm nodejs-legacy ruby ruby-sass 
     #sudo wget -qO- https://get.docker.com/ | sh
     cd /home/vagrant
     virtualenv /home/vagrant/env_joulupukki
     source /home/vagrant/env_joulupukki/bin/activate

     # Install of joulupukki-common
     git clone https://github.com/jlpk/joulupukki-common
     cd joulupukki-common
     pip install -r requirements.txt
     python setup.py develop

     # Install of joulupukki-worker
     cd /home/vagrant
     git clone https://github.com/jlpk/joulupukki-worker 
     cd joulupukki-worker
     pip install -r requirements.txt
     python setup.py develop

     # Install of joulupukki-api
     cd /home/vagrant
     git clone https://github.com/jlpk/joulupukki-api 
     cd joulupukki-api
     pip install -r requirements.txt
     python setup.py develop

     # Install of joulupukki-dispatcher
     cd /home/vagrant
     git clone https://github.com/jlpk/joulupukki-dispatcher
     cd joulupukki-dispatcher
     pip install -r requirements.txt
     python setup.py develop


     cd /home/vagrant
     git clone https://github.com/jlpk/joulupukki-web 
     source /home/vagrant/joulupukki-web/dev_virtualenv
     cd /home/vagrant/joulupukki-web && npm install grunt-cli
     cd /home/vagrant/joulupukki-web && npm install
     cd /home/vagrant/joulupukki-web && node_modules/bower/bin/bower install --allow-root -q
     cd /home/vagrant/joulupukki-web && node_modules/grunt-cli/bin/grunt build

   SHELL
end

# -*- mode: ruby -*-
# vi: set ft=ruby :

##########################################################################################
# Khalid Zoabi <khalid@echelon.solutions>                                                #
# MAKE SURE VAGRANT IS RUNNING IN AN ELEVATED COMMAND PROMPT (RUN AS ADMINISTRATOR)      #
##########################################################################################

Vagrant.configure("2") do |config|

  config.vm.box = "ubuntu/trusty64"
  config.vm.network "forwarded_port", guest: 8080, host: 8080
  config.vm.synced_folder ".", "/project"

  config.vm.provider "virtualbox" do |vb|
    vb.memory = 512
    vb.cpus = 2
  end

  config.vm.provision "shell", inline: <<-SHELL

    echo "Install node"
    sudo apt-get update
    curl -sL https://deb.nodesource.com/setup_6.x | sudo -E bash -
    sudo apt-get install -y nodejs
    sudo apt-get install -y build-essential

    echo "Disable bin-links in npm (Windows experiences symlink issues otherwise)"
    npm config set bin-links false

    echo "Install swagger-node"
    cd /home/vagrant/
    npm install swagger

    echo "Get the guest VM variables (executable, IP address)"
    SWAGGER_PROJECT_EXECUTABLE="/home/vagrant/node_modules/.bin/swagger"
    SWAGGER_PROJECT_HOST="$(ifconfig eth0 | grep "inet addr" | cut -d ':' -f 2 | cut -d ' ' -f 1)"
    SWAGGER_PROJECT_PORT="8080"

    echo "Set aliases for common commands"
    echo "alias swagger-create='$SWAGGER_PROJECT_EXECUTABLE project create --framework express'" >> /home/vagrant/.bashrc
    echo "alias swagger-edit='$SWAGGER_PROJECT_EXECUTABLE project edit --host $SWAGGER_PROJECT_HOST --port $SWAGGER_PROJECT_PORT'" >> /home/vagrant/.bashrc
    echo "alias swagger-serve='$SWAGGER_PROJECT_EXECUTABLE project start --host $SWAGGER_PROJECT_HOST --port $SWAGGER_PROJECT_PORT'" >> /home/vagrant/.bashrc

    echo "Install xdg-utils, since no web browser in VM"
    sudo apt-get install -y xdg-utils

    echo "Set the vagrant user as the owner of files in /project"
    sudo chown -R vagrant /project

    echo "Set the default start directory"
    echo "cd /project" >> /home/vagrant/.bashrc

  SHELL

end

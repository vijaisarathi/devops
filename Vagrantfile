# -*- mode: ruby -*-
# vi: set ft=ruby :

# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.
Vagrant.configure("2") do |config|
  # The most common configuration options are documented and commented below.
  # For a complete reference, please see the online documentation at
  # https://docs.vagrantup.com.

  # Every Vagrant development environment requires a box. You can search for
  # boxes at https://atlas.hashicorp.com/search.
  config.vm.box = "ubuntu/xenial64"
  #config.ssh.username = "ubuntu"
  #config.ssh.password = "password"
  config.vm.provider "virtualbox" do |v|
    v.name = "tomcat-util"
	v.memory = 1024
	v.cpus = 1
  end
  config.vm.provision "file", source: "tomcat/tomcat-users.xml", destination: "/tmp/tomcat-users.xml"
  config.vm.provision "file", source: "target/addressbook.war", destination: "/tmp/addressbook.war"
	

  ##config.vm.provision :shell, path: "scripts/bootstrap.sh"
  config.vm.network :forwarded_port, guest: 8080, host: 8089
  ##config.vm.network :forwarded_port, guest: 8080, host: 8089, host_ip: "127.0.0.1"
  #config.vm.synced_folder "webapps/", "/home/vagrant/webapps"

  # Disable automatic box update checking. If you disable this, then
  # boxes will only be checked for updates when the user runs
  # `vagrant box outdated`. This is not recommended.
  # config.vm.box_check_update = false

  # Create a forwarded port mapping which allows access to a specific port
  # within the machine from a port on the host machine. In the example below,
  # accessing "localhost:8080" will access port 80 on the guest machine.
  # config.vm.network "forwarded_port", guest: 80, host: 8080

  # Create a private network, which allows host-only access to the machine
  # using a specific IP.
  # config.vm.network "private_network", ip: "192.168.33.10"

  # Create a public network, which generally matched to bridged network.
  # Bridged networks make the machine appear as another physical device on
  # your network.
  # config.vm.network "public_network"

  # Share an additional folder to the guest VM. The first argument is
  # the path on the host to the actual folder. The second argument is
  # the path on the guest to mount the folder. And the optional third
  # argument is a set of non-required options.
  # config.vm.synced_folder "../data", "/vagrant_data"

  # Provider-specific configuration so you can fine-tune various
  # backing providers for Vagrant. These expose provider-specific options.
  # Example for VirtualBox:
  #
  config.vm.provider "virtualbox" do |vb|
  #   # Display the VirtualBox GUI when booting the machine
     vb.gui = true
  #
  #   # Customize the amount of memory on the VM:
     vb.memory = "2048"
  end
  #
  # View the documentation for the provider you are using for more
  # information on available options.

  # Define a Vagrant Push strategy for pushing to Atlas. Other push strategies
  # such as FTP and Heroku are also available. See the documentation at
  # https://docs.vagrantup.com/v2/push/atlas.html for more information.
  # config.push.define "atlas" do |push|
  #   push.app = "YOUR_ATLAS_USERNAME/YOUR_APPLICATION_NAME"
  # end

  # Enable provisioning with a shell script. Additional provisioners such as
  # Puppet, Chef, Ansible, Salt, and Docker are also available. Please see the
  # documentation for more information about their specific syntax and use.
  # config.vm.provision "shell", inline: <<-SHELL
  #   apt-get update
  #   apt-get install -y apache2
  # SHELL
  config.vm.provision "shell", privileged: false, run: "always", inline: <<-SHELL
    sudo apt-get update && sudo apt-get upgrade
	sudo apt-get update
	sudo apt-get install -y openjdk-8-jdk
	##echo "JAVA_HOME=$(which java)" | sudo tee -a /etc/environment
	##source /etc/environment
	##echo $JAVA_HOME
	sudo mkdir /opt/tomcat
	cd /opt/tomcat
	sudo apt-get -y install curl
	sudo curl -O --progress-bar https://downloads.apache.org/tomcat/tomcat-8/v8.5.54/bin/apache-tomcat-8.5.54.tar.gz
	sudo tar xvzf apache-tomcat-8.5.54.tar.gz
	echo "export JAVA_HOME=/usr/lib/jvm/java-1.8.0-openjdk-amd64" >> ~/.bashrc
	echo "export CATALINA_HOME=/opt/tomcat/apache-tomcat-8.5.54" >> ~/.bashrc
	. ~/.bashrc
	export JAVA_HOME=/usr/lib/jvm/java-1.8.0-openjdk-amd64
	export CATALINA_HOME=/opt/tomcat/apache-tomcat-8.5.54
        sudo cp /tmp/tomcat-users.xml /opt/tomcat/apache-tomcat-8.5.54/conf/tomcat-users.xml
        sudo cp /tmp/addressbook.war /opt/tomcat/apache-tomcat-8.5.54/webapps/addressbook.war    
	sudo $CATALINA_HOME/bin/startup.sh
  SHELL
end

# -*- mode: ruby -*-
# vi: set ft=ruby :

#defaultsvmsconfigs
Vagrant.configure("2") do |config|
  config.vm.box = "ubuntu/bionic64"
  #config.vm.box_check_update = true
  #config.vbguest.auto_update = true
  config.vm.provider "virtualbox" do |config|
    config.memory = 512
    config.cpus = 2
    #config.gui = false
        
  end
  
  #apt-cacher-ng-vm
  
  $cacher = <<-SCRIPT
  #!/usr/bin/env bash
  sudo apt update -y
  sudo apt upgrade -y
  sudo apt install -y apt-cacher-ng
  sudo ufw allow 3142/tcp 
  sudo ufw reload
  sudo systemctl status apt-cacher-ng
  SCRIPT

  config.vm.define "cacher" do |host|
    host.vm.hostname = "cacher"
    host.vm.network "public_network", use_dhcp_assigned_default_route: true, bridge: "enp7s0"
    #host.vm.network "public_network", bridge: "enp7s0", auto_config: true
    #config.vm.provision "shell",
    #  run: "always",
    #  inline: "ifconfig eth1 10.2.0.31 netmask 255.255.240.0 up"
    #  inline: "route add default gw 10.2.0.1"
    #end
    host.vm.provision "shell", inline: $cacher
        
  end

  #mysql-vm
 
  $mysql = <<-SCRIPT
  #!/usr/bin/env bash
  sudo touch /etc/apt/apt.conf.d/00aptproxy
  echo "Acquire::http::Proxy "http://10.2.6.74:3142";" > /etc/apt/apt.conf.d/00aptproxy 
  sudo apt update -y
  sudo apt upgrade -y
  sudo apt install -y mysql-server-5.7 python3-mysqldb
  sudo ufw allow 3306/tcp 
  sudo ufw reload
  sed -i "s/^bind-address.*127.0.0.1/bind-address=0.0.0.0/" /etc/mysql/my.cnf
  sudo systemctl restart mysql
  sudo systemctl status mysql
  SCRIPT
  
  config.vm.define "mysql" do |mysql|
    mysql.vm.provider "virtualbox" do |config|
      config.name = "mysql"
      config.memory = 2048
      config.cpus = 2
    end
    
    mysql.vm.network "public_network", use_dhcp_assigned_default_route: true, bridge: "enp7s0"
    mysql.vm.provision "shell", inline: $mysql
    
                
  end

  #ansible-vm
  config.vm.define "ansible" do |ansible|
    ansible.vm.provider "virtualbox" do |config|
      config.name = "ansible"
      config.memory = 2048
      config.cpus = 2
    end
    #ansible.vm.network "public_network", ip: "10.2.0.211", bridge: "enp7s0"
    ansible.vm.network "public_network", ip: "dhcp", bridge: "enp7s0"
    ansible.vm.provision "shell", inline: "sudo systemctl stop firewalld && sudo systemctl disable firewalld"
  end
  
  #docker-vm
  config.vm.define "docker" do |docker|
    docker.vm.provider "virtualbox" do |config|
      config.name = "docker"
      config.memory = 2048
      config.cpus = 2
    end
    #docker.vm.network "public_network", ip: "10.2.0.212", bridge: "enp7s0"
    docker.vm.network "public_network", ip: "dhcp", bridge: "enp7s0"
    docker.vm.provision "shell", inline: "sudo yum update -y && sudo yum install -y docker.io && sudo usermod -aG docker vagrant"
    docker.vm.provision "shell", inline: "sudo systemctl stop firewalld && sudo systemctl disable firewalld"
  end


end

# -*- mode: ruby -*-
# vi: set ft=ruby :
#DefaultsConfigs
Vagrant.configure("2") do |config|
  config.vm.box = "centos/7"
  config.vm.box_check_update = true
  config.vm.provider "virtualbox" do |config|
    config.memory = 512
    config.cpus = 1
    config.gui = false

  end
  
  #MysqlVM
  config.vm.define "mysql" do |mysql|
    mysql.vm.provider "virtualbox" do |config|
      config.name = "Mysql"
      config.memory = 2048
      config.cpus = 2
    end
    mysql.vm.network "public_network", ip: "dhcp", bridge: "enp7s0"
    mysql.vm.provision "shell", inline: "sudo systemctl stop firewalld && sudo systemctl disable firewalld"
    mysql.vm.provision "shell", inline: "yum update -y && yum upgrade -y && yum install -y vim htop wget build-essential g++ git"
    # internet
    mysql.vm.network :forwarded_port, guest: 8080, host: 8080
    # node debugging with VS Code
    mysql.vm.network :forwarded_port, host: 5858, guest: 5858
    # mysql
    mysql.vm.network :forwarded_port, guest: 3306, host: 3306
  end

  #AnsibleVM
  config.vm.define "ansible" do |ansible|
    ansible.vm.provider "virtualbox" do |config|
      config.name = "Ansible"
      config.memory = 2048
      config.cpus = 2
    end
    #ansible.vm.network "public_network", ip: "10.2.0.211", bridge: "enp7s0"
    ansible.vm.network "public_network", ip: "dhcp", bridge: "enp7s0"
    ansible.vm.provision "shell", inline: "sudo systemctl stop firewalld && sudo systemctl disable firewalld"
  end
  
  #DockerVM
  config.vm.define "docker" do |docker|
    docker.vm.provider "virtualbox" do |config|
      config.name = "Docker"
      config.memory = 2048
      config.cpus = 2
    end
    #docker.vm.network "public_network", ip: "10.2.0.212", bridge: "enp7s0"
    docker.vm.network "public_network", ip: "dhcp", bridge: "enp7s0"
    docker.vm.provision "shell", inline: "sudo yum update -y && sudo yum install -y docker.io && sudo usermod -aG docker vagrant"
    docker.vm.provision "shell", inline: "sudo systemctl stop firewalld && sudo systemctl disable firewalld"
  end


end

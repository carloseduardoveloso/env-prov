Vagrant.configure("2") do |config|
  config.vm.box = "ubuntu/bionic64"
  config.vm.provider "virtualbox" do |config|
    config.memory = 1024
    config.cpus = 2
  end
  
  config.vm.define "ansible" do |ansible|
    ansible.vm.provider "virtualbox" do |config|
      config.name = "ubuntu_ansible"
      config.memory = 2048
    end
    ansible.vm.network "public_network", ip: "10.2.0.211", bridge: "enp7s0"
    ansible.vm.provision "shell", inline: "apt-get update && apt-get upgrade -y && apt-get install -y software-properties-common" 
    ansible.vm.provision "shell", inline: "apt-add-repository --yes --update ppa:ansible/ansible && apt-get install -y vim htop wget ansible"
    ansible.vm.provision "shell", inline: "cp /vagrant/ssh_keys/id_bionic ~/.ssh/id_bionic && chmod 600 ~/.ssh/id_bionic && chown vagrant:vagrant ~/.ssh/id_bionic"
    #ansible.vm.provision "shell", inline: "ansible-playbook -i /vagrant/ansible/hosts /vagrant/ansible/mysql_playbook.yml"
    ansible.vm.provision "shell", inline: "cat /vagrant/ssh_keys/id_bionic.pub >> /home/vagrant/.ssh/authorized_keys"
  end 

  config.vm.define "docker" do |docker|
    docker.vm.provider "virtualbox" do |config|
      config.memory = 2048
      config.cpus = 2
      config.name = "ubuntu_docker"
    end
    docker.vm.network "public_network", ip: "10.2.0.212", bridge: "enp7s0"
    docker.vm.provision "shell", inline: "apt-get update && apt-get install -y docker.io && usermod -aG docker vagrant"
    docker.vm.provision "shell", inline: "cat /vagrant/ssh_keys/id_bionic.pub >> /home/vagrant/.ssh/authorized_keys"
  end
  
  config.vm.define "mysqlserver" do |mysqlserver|
    mysqlserver.vm.provider "virtualbox" do |config|
      config.name = "ubuntu_mysqlserver"
    end
    mysqlserver.vm.network "public_network", ip: "10.2.0.213", bridge: "enp7s0"
    mysqlserver.vm.provision "shell", inline: "cat /vagrant/ssh_keys/id_bionic.pub >> /home/vagrant/.ssh/authorized_keys"
  end

  config.vm.define "jenkins" do |jenkins|
    jenkins.vm.provider "virtualbox" do |config|
      config.name = "ubuntu_jenkins"
      config.memory = 4096
    end
    jenkins.vm.network "public_network", ip: "10.2.0.214", bridge: "enp7s0"
    #jenkins.vm.network "forwarded_port", guest: 8080, host: 8080
    jenkins.vm.provision "shell", inline: "cat /vagrant/ssh_keys/id_bionic.pub >> /home/vagrant/.ssh/authorized_keys"
    jenkins.vm.provision "shell", inline: "apt-get update && apt-get upgrade -y && apt-get install -y wget vim htop"
        
  end

    
end

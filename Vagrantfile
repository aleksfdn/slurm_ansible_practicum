# -*- mode: ruby -*-
# vi: set ft=ruby :
 
# All Vagrant configyuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.
Vagrant.configure("2") do |config|

  config.vm.provision "file", source: "ssh-keys/ansible.pub", destination: "/home/vagrant/.ssh/"

  config.vm.define "controlnode" do |controlnode|
    
    controlnode.vm.provider "virtualbox" do |vb|
      vb.name = "ansible_controlenode_ubuntu"
      # Display the VirtualBox GUI when booting the machine
      vb.gui = false
      # Customize the amount of memory on the VM:
      vb.memory = "4096"
      vb.cpus = 1
    end
    
    controlnode.vm.box = "ubuntu_20.04"
    controlnode.vm.hostname = "controlnode"
    controlnode.vm.network "private_network", ip: "192.168.60.104"
    controlnode.vm.synced_folder "./ansible","/home/vagrant/ansible"
    controlnode.vm.synced_folder "./ssh-keys","/home/vagrant/ssh-keys/"
    controlnode.vm.provision "file", source: "ssh-keys/ansible", destination: "/home/vagrant/.ssh/"
    controlnode.vm.provision "shell", inline: <<-SHELL
     sed -i 's/ChallengeResponseAuthentication no/ChallengeResponseAuthentication yes/#g' /etc/ssh/sshd_config
     service ssh restart
     chmod 600 /home/vagrant/.ssh/ansible
     chmod 644 /home/vagrant/.ssh/ansible.pub 
     cat /home/vagrant/.ssh/ansible.pub >> /home/vagrant/.ssh/authorized_keys
     apt update -y
     apt install -y ansible
     apt install -y mc
     sudo -u vagrant sh -c "ansible-galaxy collection install community.postgresql ansible.posix"
     sudo -u vagrant sh -c "ansible-galaxy role install geerlingguy.nginx"
    SHELL
  end
  
  config.vm.define "server" do |server|
    server.vm.provider "virtualbox" do |vb|
      vb.name = "ansible_server_centos"
      # Display the VirtualBox GUI when booting the machine
      vb.gui = false
      # Customize the amount of memory on the VM:
      vb.memory = "4096"
      vb.cpus = 1
    end
    server.vm.box = "bento/centos-7.9"
    server.vm.hostname = "server"
    #server.vbguest.auto_update = false
    server.vm.network "private_network", ip: "192.168.60.105"
    server.vm.provision "shell", inline: <<-SHELL
     chmod 644 /home/vagrant/.ssh/ansible.pub
     cat /home/vagrant/.ssh/ansible.pub >> /home/vagrant/.ssh/authorized_keys
     yum update -y
     yum install -y mc
    SHELL
  end
 end
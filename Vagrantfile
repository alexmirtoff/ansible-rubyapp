# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|

  config.vm.provision "file", source: "files/vagrant.pub", destination: "/home/vagrant/.ssh/"

  config.vm.define "controlnode" do |controlnode|
    controlnode.vm.box = "ubuntu/focal64"
    controlnode.vm.hostname = "controlnode"
    controlnode.vm.network "private_network", ip: "192.168.56.200"
    controlnode.vm.synced_folder "./ansible","/home/vagrant/ansible"
    controlnode.vm.synced_folder "./files","/home/vagrant/files/"
    controlnode.vm.provision "file", source: "files/vagrant", destination: "/home/vagrant/.ssh/"
    controlnode.vm.provision "shell", inline: <<-SHELL
      sed -i 's/ChallengeResponseAuthentication no/ChallengeResponseAuthentication yes/#g' /etc/ssh/sshd_config
      service ssh restart
      sudo add-apt-repository ppa:ansible/ansible
      sudo apt update -y && sudo apt -y install sshpass ansible
      chmod 600 /home/vagrant/.ssh/vagrant
      chmod 644 /home/vagrant/.ssh/vagrant.pub
    SHELL
  end

  config.vm.define "ruby_server" do |ruby_server|
    ruby_server.vm.box = "bento/centos-7.9"
    ruby_server.vm.hostname = "ruby-server"
    ruby_server.vm.network "private_network", ip: "192.168.56.210"
    ruby_server.vm.provision "shell", inline: <<-SHELL
      chmod 644 /home/vagrant/.ssh/vagrant.pub
      cat /home/vagrant/.ssh/vagrant.pub >> /home/vagrant/.ssh/authorized_keys
    SHELL
  end

end

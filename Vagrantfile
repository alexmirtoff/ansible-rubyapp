# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|

  config.vm.provision "file", source: "files/vagrant.pub", destination: "/home/vagrant/.ssh/"

  config.vm.define "ruby_server" do |ruby_server|
    ruby_server.vm.box = "bento/centos-7.9"
    ruby_server.vm.hostname = "ruby-server"
    ruby_server.vm.network "private_network", ip: "192.168.56.210"
    ruby_server.vm.provision "shell", inline: <<-SHELL
      chmod 644 /home/vagrant/.ssh/vagrant.pub
      cat /home/vagrant/.ssh/vagrant.pub >> /home/vagrant/.ssh/authorized_keys
    SHELL
  end

  config.vm.define "controlnode" do |controlnode|
    controlnode.vm.box = "ubuntu/focal64"
    controlnode.vm.hostname = "controlnode"
    controlnode.vm.network "private_network", ip: "192.168.56.200"
    controlnode.vm.synced_folder "./ansible","/home/vagrant/ansible"
    controlnode.vm.synced_folder "./files","/home/vagrant/files/"
    controlnode.vm.provision "file", source: "files/vagrant", destination: "/home/vagrant/.ssh/"
    controlnode.vm.provision "shell", keep_color: true, inline: <<-SHELL
      sed -i 's/ChallengeResponseAuthentication no/ChallengeResponseAuthentication yes/#g' /etc/ssh/sshd_config
      service ssh restart
      sudo add-apt-repository ppa:deadsnakes/ppa -y
      sudo apt update -y && sudo apt -y install sshpass
      sudo apt install python3.11 -y
      sudo apt install python3.11-full -y
      wget https://bootstrap.pypa.io/get-pip.py
      sudo python3.11 get-pip.py
      sudo python3.11 -m pip install --upgrade pip
      sudo update-alternatives --install /usr/bin/python python /usr/bin/python3.11 2
      sudo update-alternatives --install /usr/bin/python python /usr/bin/python3.8 1
      sudo update-alternatives --install /usr/bin/python3 python3 /usr/bin/python3.11 2
      sudo update-alternatives --install /usr/bin/python3 python3 /usr/bin/python3.8 1
      sudo -u vagrant python3 -m pip install --user pipx
      sudo -u vagrant python3 -m pipx ensurepath
      sudo -u vagrant /home/vagrant/.local/bin/pipx install ansible-core==2.16.2
      chmod 600 /home/vagrant/.ssh/vagrant
      chmod 644 /home/vagrant/.ssh/vagrant.pub
    SHELL
  end

end

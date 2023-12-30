# ansible-rubyapp
This repository was created for practice.
This set of tools will install xPaste written on Ruby: https://github.com/southbridgeio/xpaste

Tested with:

Vagrant 2.4.0<br />
Ansible-core 2.16.2<br />
Ruby 2.3.8<br />
PostgreSQL 14<br />
Nginx 1.24.0<br />

Howto use:

1. Generate SSH keys with name vagrant/vagrant.pub and put them to files directory.
2. vagrant up
3. vagrant ssh controlnode
4. cd ~/ansible
5. ansible-galaxy install -r requirements.yml
6. ansible-playbook playbook.yml

Or you can use it without controlnode & manage it from your host.
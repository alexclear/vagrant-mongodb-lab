# -*- mode: ruby -*-
# vi: set ft=ruby :

$MONGO1_IP = "172.16.137.82"
$MONGO2_IP = "172.16.137.83"
$MONGO3_IP = "172.16.137.84"

ANSIBLE_RAW_SSH_ARGS = []

ANSIBLE_RAW_SSH_ARGS << " -o IdentityFile=./.vagrant/machines/mongo1/virtualbox/private_key "
ANSIBLE_RAW_SSH_ARGS << " -o IdentityFile=./.vagrant/machines/mongo2/virtualbox/private_key "
ANSIBLE_RAW_SSH_ARGS << " -o IdentityFile=./.vagrant/machines/mongo3/virtualbox/private_key "

Vagrant.configure("2") do |config|
  config.vm.define "mongo1" do |mongo1|
    mongo1.vm.box = "ubuntu/bionic64"
    mongo1.vm.hostname = "mongo1"
    mongo1.vm.network "private_network", ip: $MONGO1_IP
    mongo1.vm.network "forwarded_port", guest: 9090, host: 9091
    mongo1.vm.network "forwarded_port", guest: 3000, host: 3001

    mongo1.vm.provider :virtualbox do |v, override|
      v.gui = false
      v.customize ["modifyvm", :id, "--cpus", 2]
      v.customize ["modifyvm", :id, "--memory", 4096]
      v.customize ["modifyvm", :id, "--cableconnected1", "on"]
      v.customize ["modifyvm", :id, "--cableconnected2", "on"]
    end

    mongo1.vm.provision "shell", inline: "apt-get install -y python"
  end

  config.vm.define "mongo2" do |mongo2|
    mongo2.vm.box = "ubuntu/bionic64"
    mongo2.vm.hostname = "mongo2"
    mongo2.vm.network "private_network", ip: $MONGO2_IP

    mongo2.vm.provider :virtualbox do |v, override|
      v.gui = false
      v.customize ["modifyvm", :id, "--cpus", 2]
      v.customize ["modifyvm", :id, "--memory", 4096]
      v.customize ["modifyvm", :id, "--cableconnected1", "on"]
      v.customize ["modifyvm", :id, "--cableconnected2", "on"]
    end

    mongo2.vm.provision "shell", inline: "apt-get install -y python"
  end

  config.vm.define "mongo3" do |mongo3|
    mongo3.vm.box = "ubuntu/bionic64"
    mongo3.vm.hostname = "mongo3"
    mongo3.vm.network "private_network", ip: $MONGO3_IP

    mongo3.vm.provider :virtualbox do |v, override|
      v.gui = false
      v.customize ["modifyvm", :id, "--cpus", 2]
      v.customize ["modifyvm", :id, "--memory", 4096]
      v.customize ["modifyvm", :id, "--cableconnected1", "on"]
      v.customize ["modifyvm", :id, "--cableconnected2", "on"]
    end

    mongo3.vm.provision "shell", inline: "apt-get install -y python"
    mongo3.vm.provision "ansible" do |ansible|
      ansible.playbook = "ansible/site.yml"
      ansible.config_file = "ansible/ansible.cfg"
      ansible.inventory_path = "ansible/inventory"
      ansible.become = true
      ansible.limit = "all"
      ansible.raw_ssh_args = ANSIBLE_RAW_SSH_ARGS
    end
  end
end

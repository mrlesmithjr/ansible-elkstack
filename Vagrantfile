# -*- mode: ruby -*-
# vi: set ft=ruby :

# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.
Vagrant.configure(2) do |config|
  config.vm.define "elkbroker" do |elkbroker|
    elkbroker.vm.box = "mrlesmithjr/trusty64"
    elkbroker.vm.hostname = "elkbroker"

    elkbroker.vm.network :private_network, ip: "192.168.202.200"

    elkbroker.vm.provider "virtualbox" do |vb|
      vb.memory = "1024"
    end
    elkbroker.vm.provision :shell, path: "provision.sh", keep_color: "true"
    elkbroker.vm.provision :shell, inline: 'ansible-galaxy install -r /vagrant/requirements.yml -f'
    elkbroker.vm.provision :shell, inline: 'ansible-playbook -i /vagrant/hosts -c local /vagrant/elkstack-core.yml --limit "elk-broker-nodes"'
    elkbroker.vm.provision :shell, inline: 'ansible-playbook -i /vagrant/hosts -c local /vagrant/elkstack.yml --limit "elk-broker-nodes"'
  end
  config.vm.define "elkes" do |elkes|
    elkes.vm.box = "mrlesmithjr/trusty64"
    elkes.vm.hostname = "elkes"

    elkes.vm.network :private_network, ip: "192.168.202.201"
    elkes.vm.network "forwarded_port", guest: 9200, host: 9200

    elkes.vm.provider "virtualbox" do |vb|
      vb.memory = "2048"
    end
    elkes.vm.provision :shell, path: "provision.sh", keep_color: "true"
    elkes.vm.provision :shell, inline: 'ansible-galaxy install -r /vagrant/requirements.yml -f'
    elkes.vm.provision :shell, inline: 'ansible-playbook -i /vagrant/hosts -c local /vagrant/elkstack-core.yml --limit "elk-es-nodes"'
    elkes.vm.provision :shell, inline: 'ansible-playbook -i /vagrant/hosts -c local /vagrant/elkstack.yml --limit "elk-es-nodes"'
  end
end

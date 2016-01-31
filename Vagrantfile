# -*- mode: ruby -*-
# vi: set ft=ruby :

# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.
Vagrant.configure(2) do |config|
  config.vm.define "elk-broker" do |elk-broker|
    elk-broker.vm.box = "mrlesmithjr/trusty64"
    elk-broker.vm.hostname = "elk-broker"

    elk-broker.vm.network :private_network, ip: "192.168.202.200"

    elk-broker.vm.provider "virtualbox" do |vb|
      vb.memory = "1024"
    end
    elk-broker.vm.provision :shell, path: "provision.sh", keep_color: "true"
    elk-broker.vm.provision :shell, inline: 'ansible-galaxy install -r /vagrant/requirements.yml -f'
    elk-broker.vm.provision :shell, inline: 'ansible-playbook -i /vagrant/hosts -c local /vagrant/elkstack-core.yml --limit "elk-broker-nodes"'
    elk-broker.vm.provision :shell, inline: 'ansible-playbook -i /vagrant/hosts -c local /vagrant/elkstack.yml --limit "elk-broker-nodes"'
  end
  config.vm.define "elk-es" do |elk-es|
    elk-es.vm.box = "mrlesmithjr/trusty64"
    elk-es.vm.hostname = "elk-es"

    elk-es.vm.network :private_network, ip: "192.168.202.201"
    elk-es.vm.network "forwarded_port", guest: 9200, host: 9200

    elk-es.vm.provider "virtualbox" do |vb|
      vb.memory = "2048"
    end
    elk-es.vm.provision :shell, path: "provision.sh", keep_color: "true"
    elk-es.vm.provision :shell, inline: 'ansible-galaxy install -r /vagrant/requirements.yml -f'
    elk-es.vm.provision :shell, inline: 'ansible-playbook -i /vagrant/hosts -c local /vagrant/elkstack-core.yml --limit "elk-es-nodes"'
    elk-es.vm.provision :shell, inline: 'ansible-playbook -i /vagrant/hosts -c local /vagrant/elkstack.yml --limit "elk-es-nodes"'
  end
end

# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|

  config.vm.define "control01" do |webtier|
      webtier.vm.box = "bento/ubuntu-18.10"
      webtier.vm.hostname = "control01"
      webtier.vm.network "private_network", ip: "192.168.45.10"
      webtier.vm.provider "virtualbox" do |vb|
          vb.name = "control01"
          vb.gui = false
          vb.memory = "1024"
          vb.cpus = 1
      end
      webtier.vm.provision "ansible_local" do |ansible|
      ansible.compatibility_mode = "2.0"
      ansible.version = "2.8.2"
      ansible.playbook = "deploy.yml"
      end

    end

    config.vm.define "control02" do |webtier|
        webtier.vm.box = "bento/ubuntu-18.10"
        webtier.vm.hostname = "control02"
        webtier.vm.network "private_network", ip: "192.168.45.11"
        webtier.vm.provider "virtualbox" do |vb|
            vb.name = "control02"
            vb.gui = false
            vb.memory = "512"
            vb.cpus = 1
        end
        webtier.vm.provision "ansible_local" do |ansible|
        ansible.compatibility_mode = "2.0"
        ansible.version = "2.8.2"
        ansible.playbook = "deploy.yml"
        end

      end


end

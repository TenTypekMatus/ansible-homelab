# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
  

VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  config.vm.box = "almalinux/9"
  config.vm.synced_folder '.', '/vagrant', disabled: true
  config.ssh.insert_key = false

  config.vm.provider :virtualbox do |v|
    v.memory = 4096
    v.cpus = 2
    v.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
    v.customize ["modifyvm", :id, "--ioapic", "on"]
  end

  # 1st server.
  config.vm.define "logs" do |logs|
    logs.vm.hostname = "logs.test"
    logs.vm.network :private_network, ip: "192.168.56.90"

    logs.vm.provision :ansible do |ansible|
      ansible.playbook = "site.yml"
      ansible.inventory_path = "inventory.yml"
      ansible.become = true
    end
  end

  # 2nd server.
  config.vm.define "web" do |web|
    web.vm.hostname = "web.test"
    web.vm.network :private_network, ip: "192.168.56.91"

    web.vm.provider :virtualbox do |v|
      v.memory = 512
      v.cpus = 1
    end

    web.vm.provision :ansible do |ansible|
      ansible.playbook = "site.yml"
      ansible.inventory_path = "./inventory.yml"
      ansible.become = true
    end
  end
end
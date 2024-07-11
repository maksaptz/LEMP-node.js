# -*- mode: ruby -*-
# vim: set ft=ruby :
#Path to HDD on host

Vagrant.configure(2) do |config|
  config.vm.provider "virtualbox" do |v|
    v.memory = 4096
    v.cpus = 2
  end

  config.vm.define "web" do |host|
    host.vm.box = "debian/bullseye64"
    host.vm.box_version = "11.20230615.1"
    host.vm.hostname = "web"
  host.vm.network "private_network",
                     ip: "192.168.50.10",
                     adapter: 4
  end

#   host.vm.provision "ansible" do |ansible|
#      ansible.playbook = "ansible/play.yml"
#      ansible.inventory_path = "ansible/hosts"
#      ansible.host_key_checking = "false"
#      ansible.limit = "all"
#   end

end

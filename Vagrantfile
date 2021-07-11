# -*- mode: ruby -*-
# vi: set ft=ruby :

# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.
Vagrant.configure("2") do |config|
  # The most common configuration options are documented and commented below.
  # For a complete reference, please see the online documentation at
  # https://docs.vagrantup.com.

  # Every Vagrant development environment requires a box. You can search for
  # boxes at https://vagrantcloud.com/search.
  config.vm.box = "hashicorp/bionic64"


  (1..2).each do |i|
    hostname = "worker#{i}"
    config.vm.define hostname do |worker|
      worker.vm.network "private_network", 
      ip: "192.168.123.#{1 + i}"
      worker.vm.hostname = hostname
    end
  end

  config.vm.define "master" do |master|
    master.vm.network "private_network", 
    ip: "192.168.123.4"
    master.vm.hostname = "master"
    master.vm.provision "ansible" do |ansible|
      ansible.playbook = "master_worker_setup.yml"
      ansible.groups = {
        "master_node" => ["master"],
        "worker_nodes" => ["worker[1:2]"],
        "all" => ["master", "worker[1:2]"]
      }
      ansible.limit = "all"
      # ansible.verbose = "vvv"
      ansible.extra_vars = {
        ips: {

        }
      }
  end
  end

  config.vm.provision "ansible" do |ansible|
    ansible.playbook = "playbook.yml"
    ansible.groups = {
      "master_node" => ["master"],
      "worker_nodes" => ["worker[1:2]"],
    }
  end
end

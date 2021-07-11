# -*- mode: ruby -*-
# vi: set ft=ruby :

NUM_WORKERS = 2

VM_CONFIGS = [
  {name: "master", ip: "192.168.123.4"},
]

(1..NUM_WORKERS).each do |i|
  VM_CONFIGS.append(
    {name: "worker#{i}", ip: "192.168.123.#{i+1}"},
  )
end

GROUPS = {
  "master_node" => ["master"],
  "worker_nodes" => ["worker[1:#{NUM_WORKERS}]"],
  "all" => ["master", "worker[1:#{NUM_WORKERS}]"]
}

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


  VM_CONFIGS.each_with_index do |cfg, idx|
    hostname = cfg[:name]
    ip = cfg[:ip]
    config.vm.define hostname do |machine|
      machine.vm.network "private_network", 
      ip: ip
      machine.vm.hostname = hostname

      if idx == VM_CONFIGS.length - 1 then
        machine.vm.provision "ansible" do |ansible|
          ansible.playbook = "master_worker_setup.yml"
          ansible.groups = GROUPS
          ansible.limit = "all"
        end
      end
    end
  end


  config.vm.provision "ansible" do |ansible|
    ansible.playbook = "playbook.yml"
    ansible.groups = GROUPS
  end
end

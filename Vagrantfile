# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
  config.vm.box = "bento/ubuntu-18.04"
  host_vars = {}
  cluster_size = 3 # number of nodes, first node will become replication master, 2..N will become replication slaves
  master_node_id = 1 # changing this shoud result in moving master seamlessly
  config.vm.provider "virtualbox" do |vb|
    vb.memory = "512"
  end

  (1..cluster_size).each do |node_id|
    config.vm.define "node-#{node_id}" do |node|
      node.vm.network "private_network", ip: "192.168.22.#{10+node_id}"
      node.vm.hostname = "node-#{node_id}"
      host_vars["node-#{node_id}"] = {
        "server_id" => node_id
      }
      # define first node as master, rest of nodes - as slaves
      if node_id == master_node_id
        host_vars["node-#{node_id}"]["replication_role"] = "master"
      else
        host_vars["node-#{node_id}"]["replication_role"] = "slave"
      end

      # Only execute once the Ansible provisioner,
      # when all the machines are up and ready.
      if node_id == cluster_size
        node.vm.provision :ansible do |ansible|
          # Disable default limit to connect to all the machines
          ansible.limit = "all"
          ansible.playbook = "provisioning/main.yml"
          ansible.host_vars = host_vars
        end
      end
    end
  end

end

# Example mysql master-slave replication role

This role aims to rollout mysql clusters consisting of one master and mutliple slaves, as shown on the below diagram:

```
         +---------------+
         |node-1 (master)|
         +---------------+
           /          \
+--------------+   +--------------+
|node-2 (slave)|   |node-3 (slave)|
+--------------+   +--------------+
```

The role is also capable of switching masters as described in https://dev.mysql.com/doc/refman/5.7/en/replication-solutions-switch.html, provided that no new requests are coming to mysql master during `ansible-playbook` runs.

## Test using Vagrant

```
vagrant up
```

## Test using Ansible
```
ansible -i .vagrant/provisioners/ansible/inventory/vagrant_ansible_inventory -m ping all
```

## Re-provision using Vagrant:
```
vagrant provision
```

## Re-provision using Ansible:
```
ansible-playbook -i .vagrant/provisioners/ansible/inventory/vagrant_ansible_inventory provisioning/main.yml
```

## Vagrant cluster configuration can be ajusted by changing following variablas in Vagrantfile:
```
  cluster_size = 3 # number of nodes, first node will become replication master, 2..N will become replication slaves
  master_node_id = 2 # changing this shoud result in moving master seamlessly
```

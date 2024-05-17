# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
  (1..3).each do |i|
    config.vm.define "node#{i}" do |node|
      node.vm.box = "debian/bookworm64"
      node.vm.hostname = "node#{i}"
      node.vm.network "private_network", ip: "192.168.33.#{i}"
      node.vm.provider "virtualbox" do |vb|
        vb.memory = "2048"
        vb.cpus = 2
      end
    end
  end

  config.vm.define "node1" do |node|
    # Ansible 
    node.vm.provision "ansible_local" do |ansible|
      ansible.playbook = "/vagrant/provision/playbook.yaml"
      ansible_groups = {
        "kubernetes" => ["node1", "node2", "node3"],
        "kubernetes_master" => ["node1"],
        "kubernetes_node" => ["node2", "node3"],
      }
      ansible.inventory_path = "/vagrant/provision/inventory"
    end
  end
end

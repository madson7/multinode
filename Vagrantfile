# -*- mode: ruby -*-
# vi: set ft=ruby :

# Configuration section --------------------------------------------------------

IMAGE_BOX = "generic/ubuntu2004" # Ubuntu or debian
NODE_CPUS = 4  # vCPUs per Cloud Node
NODE_MEMORY = 6144  # RAM per Cloud Node
MASTER_NODES_COUNT = 1
CLIENT_NODES_COUNT = 1

def packages_debianoid(user)
  return <<-EOF
    export DEBIAN_FRONTEND=noninteractive
    apt-get update
  EOF
end

Vagrant.configure("2") do |config|

  config.ssh.insert_key = false
  config.ssh.private_key_path = ["~/.ssh/id_rsa", "~/.vagrant.d/insecure_private_key"]
  config.vm.provision "file", source: "~/.ssh/id_rsa.pub", destination: "~/.ssh/authorized_keys"
  config.vm.provision "packages debianoid", :type => :shell, :inline => packages_debianoid("vagrant")

  (1.. MASTER_NODES_COUNT).each do |i|
    config.vm.define "master#{"%02d" % i}" do |master|
      master.vm.hostname = "master#{"%02d" % i}"
      master.vm.box = IMAGE_BOX
      master.vm.provider :libvirt do |domain|
        domain.cpus = NODE_CPUS
        domain.memory = NODE_MEMORY
        end
      end
    end

  (1.. CLIENT_NODES_COUNT).each do |i|
  config.vm.define "node#{"%02d" % i}" do |node|
    node.vm.hostname = "node#{"%02d" % i}"
    node.vm.box = IMAGE_BOX
    node.vm.provider :libvirt do |domain|
      domain.cpus = NODE_CPUS
      domain.memory = NODE_MEMORY
      end
    end
  end
end
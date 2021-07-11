ENV['VAGRANT_DEFAULT_PROVIDER'] = "virtualbox"
Vagrant.configure("2") do |config|
    config.vm.provision "shell", inline: <<-SHELL
        apt-get update -y
        echo "10.0.0.10  control-plane" >> /etc/hosts
        echo "10.0.0.11  node01" >> /etc/hosts
        echo "10.0.0.12  node02" >> /etc/hosts
    SHELL
    
    config.vm.define "control-plane", primary: true  do |master|
      master.vm.box = "bento/ubuntu-18.04"
      master.vm.hostname = "control-plane"
      master.vm.network "private_network", ip: "10.0.0.10"
      master.vm.provider "virtualbox" do |vb|
          vb.memory = 2048
          vb.cpus = 2
      end
      master.vm.provision "shell", path: "scripts/common.sh"
      master.vm.provision "shell", path: "scripts/cp.sh"
    end

    (1..2).each do |i|
  
    config.vm.define "node0#{i}" do |node|
      node.vm.box = "bento/ubuntu-18.04"
      node.vm.hostname = "node0#{i}"
      node.vm.network "private_network", ip: "10.0.0.1#{i}"
      node.vm.provider "virtualbox" do |vb|
          vb.memory = 2048
          vb.cpus = 1
      end
      node.vm.provision "shell", path: "scripts/common.sh"
      node.vm.provision "shell", path: "scripts/node.sh"
    end
    
    end
  end

Vagrant.configure("2") do |config|
  config.vm.box = "debian/buster64"
  
  config.vm.define "controller0" do |b|
    b.vm.provider "virtualbox" do |v|
      v.name = "k8s-controller0"
      v.cpus = "2"
      v.memory = "1024"
    end
    b.vm.network "private_network", ip: "10.240.0.10", virtualbox__intnet: "k8s-cluster"
    b.vm.hostname = "controller0"
  end
  
  config.vm.define "worker0" do |b|
    b.vm.provider "virtualbox" do |v|
      v.name = "k8s-worker0"
      v.cpus = "2"
      v.memory = "1024"
    end
    b.vm.network "private_network", ip: "10.240.0.20", virtualbox__intnet: "k8s-cluster"
    b.vm.hostname = "worker0"
  end
  
  config.vm.define "worker1" do |b|
    b.vm.provider "virtualbox" do |v|
      v.name = "k8s-worker1"
      v.cpus = "2"
      v.memory = "1024"
    end
    b.vm.network "private_network", ip: "10.240.0.21", virtualbox__intnet: "k8s-cluster"
    b.vm.hostname = "worker1"
  end    
  
  config.vm.define "director", primary: true do |b|
    b.vm.provider "virtualbox" do |v|
      v.name = "k8s-director"
      v.cpus = "2"
      v.memory = "1024"
    end
    b.vm.network "private_network", ip: "10.240.0.2", virtualbox__intnet: "k8s-cluster"
    b.vm.hostname = "director"
    
    b.vm.provision "file", source: ".vagrant//machines//controller0//virtualbox//private_key", destination: "/home/vagrant/.ssh/controller0.id_rsa"
    b.vm.provision "shell", inline: "chmod 600 /home/vagrant/.ssh/controller0.id_rsa"
    b.vm.provision "file", source: ".vagrant//machines//worker0//virtualbox//private_key", destination: "/home/vagrant/.ssh/worker0.id_rsa"
    b.vm.provision "shell", inline: "chmod 600 /home/vagrant/.ssh/worker0.id_rsa"
    b.vm.provision "file", source: ".vagrant//machines//worker1//virtualbox//private_key", destination: "/home/vagrant/.ssh/worker1.id_rsa"
    b.vm.provision "shell", inline: "chmod 600 /home/vagrant/.ssh/worker1.id_rsa"

    b.vm.provision "ansible_local" do |ansible|
      ansible.provisioning_path = "/vagrant/ansible_local"
      ansible.playbook = "director.yaml"
      ansible.verbose = true
      ansible.limit = "all"
      ansible.inventory_path = "inventory.ini"
    end
  end
end
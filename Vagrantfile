Vagrant.configure("2") do |config|
  CONTROLLER_COUNT = (ENV["CONTROLLER_COUNT"] || 1).to_i
  WORKER_COUNT = (ENV["WORKER_COUNT"] || 2).to_i

  host_vars = {
    "director" => {
      "ansible_connection" => "local",
      "cluster_ip" => "10.240.0.2"
    }
  }

  config.vm.box = "debian/buster64"

  (0..CONTROLLER_COUNT-1).each do |i|
    hostname = "controller#{i}"
    host_vars = host_vars.merge(Hash[hostname, {"cluster_ip" => "10.240.0.#{10+i}"}])

    config.vm.define "#{hostname}" do |b|
      b.vm.provider "virtualbox" do |v|
        v.name = "k8s-#{hostname}"
        v.cpus = "2"
        v.memory = "1024"
      end
      b.vm.network "private_network", ip: host_vars[hostname]["cluster_ip"], virtualbox__intnet: "k8s-cluster"
      b.vm.hostname = hostname
    end
  end

  (0..WORKER_COUNT-1).each do |i|
    hostname = "worker#{i}"
    host_vars = host_vars.merge(Hash[hostname, {"cluster_ip" => "10.240.0.#{20+i}"}])

    config.vm.define "#{hostname}" do |b|
      b.vm.provider "virtualbox" do |v|
        v.name = "k8s-#{hostname}"
        v.cpus = "2"
        v.memory = "1024"
      end
      b.vm.network "private_network", ip: host_vars[hostname]["cluster_ip"], virtualbox__intnet: "k8s-cluster"
      b.vm.hostname = hostname
    end
  end

  config.vm.define "director", primary: true do |b|
    b.vm.provider "virtualbox" do |v|
      v.name = "k8s-director"
      v.cpus = "2"
      v.memory = "1024"
    end
    b.vm.network "private_network", ip: "10.240.0.2", virtualbox__intnet: "k8s-cluster"
    b.vm.hostname = "director"

    (0..CONTROLLER_COUNT-1).each do |i|
      b.vm.provision "file", source: ".vagrant//machines//controller#{i}//virtualbox//private_key", destination: "/home/vagrant/.ssh/controller#{i}.id_rsa"
      b.vm.provision "shell", inline: "chmod 600 /home/vagrant/.ssh/controller#{i}.id_rsa"
    end

    (0..WORKER_COUNT-1).each do |i|
      b.vm.provision "file", source: ".vagrant//machines//worker#{i}//virtualbox//private_key", destination: "/home/vagrant/.ssh/worker#{i}.id_rsa"
      b.vm.provision "shell", inline: "chmod 600 /home/vagrant/.ssh/worker#{i}.id_rsa"
    end

    b.vm.provision "ansible_local" do |ansible|
      ansible.provisioning_path = "/vagrant/ansible/provision_vms"
      ansible.playbook = "director.yaml"
      ansible.verbose = true
      ansible.limit = "all"
      ansible.groups = {
        "directors" => ["director"],
        "controllers" => (0..CONTROLLER_COUNT-1).to_a.map {|n| "controller#{n}"}.compact,
        "workers" => (0..WORKER_COUNT-1).to_a.map {|n| "worker#{n}"}.compact,
      }
      ansible.host_vars = host_vars
    end

    b.vm.provision "shell", inline: "cd /vagrant/ansible/deploy_k8s_cluster && ansible-playbook -i inventory.ini playbook.yaml", privileged: false
  end
end
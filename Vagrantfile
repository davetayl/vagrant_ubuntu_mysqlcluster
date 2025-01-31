Vagrant.configure("2") do |config|
  config.vm.define "primary" do |primary|
      primary.vm.box = "ubuntu/focal64"
      primary.vm.provider "virtualbox" do |vb|
          vb.memory = 2048
          vb.cpus = 2
      end
    primary.vm.hostname = "primary"
    primary.vm.network "private_network", ip: "10.0.0.16", netmask:"255.255.255.0"
    primary.vm.network "forwarded_port", guest: 3306, host: 63306
    primary.vm.provision "ansible" do |ansible|
      ansible.playbook = "provisioning/primary.yml"
    end
  end
  (1..3).each do |i|
    config.vm.define "worker#{i}" do |worker|
      worker.vm.box = "ubuntu/focal64"
      worker.vm.provider "virtualbox" do |vb|
          vb.memory = 2048
          vb.cpus = 2
      end
      worker.vm.hostname = "worker#{i}"
      worker.vm.network "private_network", ip: "10.0.0.#{17+i}", netmask:"255.255.255.0"
      worker.vm.network "forwarded_port", guest: 3306, host: "#{3306+i}"
      worker.vm.provision "ansible" do |ansible|
        ansible.playbook = "provisioning/worker.yml"
        ansible.extra_vars = {
          worker_id: "#{i}"
        }
      end
    end
  end
end
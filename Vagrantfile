Vagrant.configure("2") do |config|
  config.vm.define "master" do |master|
      master.vm.box = "debian/bullseye64"
      master.vm.provider "virtualbox" do |vb|
          vb.memory = 2048
          vb.cpus = 2
      end
    master.vm.hostname = "master"
    master.vm.network "private_network", ip: "10.0.0.16", netmask:"255.255.255.0"
    master.vm.network "forwarded_port", guest: 3306, host: 63306
    master.vm.provision "ansible" do |ansible|
      ansible.playbook = "provisioning/master.yml"
    end
  end
  (1..3).each do |i|
    config.vm.define "worker#{i}" do |worker|
      worker.vm.box = "debian/bullseye64"
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
WORKERS_NUMBER = 2

Vagrant.configure("2") do |config|
    config.ssh.insert_key = false
    
      
    config.vm.define "master" do |master|

        master.vm.provider "virtualbox" do |vb|
            vb.name = "master"
            vb.memory = 2048
            vb.cpus = 2
        end

        master.vm.box = "bento/ubuntu-16.04"
        master.vm.network "private_network", ip: "192.168.50.15"
        master.vm.hostname = "master"
        master.vm.provision "ansible_local" do |ansible|
            ansible.playbook = "kubernetes-setup/master-playbook.yml"
            ansible.extra_vars = {
                node_ip: "192.168.50.15",
            }
        end
    end

    (1..WORKERS_NUMBER).each do |i|
        config.vm.define "worker-#{i}" do |node|

            node.vm.provider "virtualbox" do |vb|
                vb.name = "worker-#{i}"
                vb.memory = 1024
                vb.cpus = 2
            end
            node.vm.box = "bento/ubuntu-16.04"
            node.vm.network "private_network", ip: "192.168.50.#{i + 15}"
            node.vm.hostname = "worker-#{i}"
            node.vm.provision "ansible_local" do |ansible|
                ansible.playbook = "kubernetes-setup/node-playbook.yml"
                ansible.extra_vars = {
                    node_ip: "192.168.50.#{i + 15}",
                }
            end
        end
    end
end
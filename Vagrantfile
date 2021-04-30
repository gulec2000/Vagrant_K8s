IMAGE_NAME = "bento/ubuntu-18.04"
N = 3

Vagrant.configure("2") do |config|
    config.ssh.insert_key = false

    config.vm.provider "virtualbox" do |v|
        v.memory = 2048
        v.cpus = 2
    end
      
    config.vm.define "c1-cp1" do |master|
        master.vm.box = IMAGE_NAME
        master.vm.network "private_network", ip: "172.16.94.10"
        master.vm.hostname = "c1-cp1"
        master.vm.provision "ansible" do |ansible|
            ansible.playbook = "kubernetes-setup/master-playbook.yml"
            ansible.extra_vars = {
                node_ip: "172.16.94.10",
            }
        end
    end

    (1..N).each do |i|
        config.vm.define "c1-node-#{i}" do |node|
            node.vm.box = IMAGE_NAME
            node.vm.network "private_network", ip: "172.16.94.#{i + 10}"
            node.vm.hostname = "c1-node-#{i}"
            node.vm.provision "ansible" do |ansible|
                ansible.playbook = "kubernetes-setup/node-playbook.yml"
                ansible.extra_vars = {
                    node_ip: "172.16.94.#{i + 10}",
                }
            end
        end
    end
end
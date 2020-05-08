Vagrant.configure("2") do |config|

    N = 3
    (1..N).each do |machine_id|
      config.vm.define "machine#{machine_id}" do |machine|

        machine.vm.box =  "bento/ubuntu-18.04"

        machine.ssh.insert_key = false
        machine.vm.provision "file", source: "./id_rsa.pub", destination: "~/.ssh/authorized_keys"
        machine.ssh.private_key_path = ["./id_rsa", "~/.vagrant.d/insecure_private_key"]
        machine.vm.hostname = "machine#{machine_id}"
        machine.vm.network "private_network", ip: "192.168.33.#{9+machine_id}"

        if machine_id == 1
          machine.vm.provider "parallels" do |prl|
            prl.cpus = "2"
            prl.memory = "2048"
          end
        end

        if machine_id > 1
          machine.vm.provider "parallels" do |prl|
            prl.cpus = "1"
            prl.memory = "1024"
          end
        end

        if machine_id == N
          machine.vm.provision :ansible do |ansible|
            ansible.playbook = "main.yml"
            ansible.inventory_path = "hosts"
            ansible.limit = "all"
          end
        end

      end
    end

end
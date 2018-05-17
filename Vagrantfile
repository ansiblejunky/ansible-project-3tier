Vagrant.configure("2") do |config|
  
    hosts = {
      "web1" => "192.168.33.11",
      "web2" => "192.168.33.12",
      "app1" => "192.168.33.13",
      "db1" => "192.168.33.14",
      "db2" => "192.168.33.15"
    }
  
    hosts.each do | name, ip|
      config.vm.define name do | machine |
        machine.vm.box = "rhel75"
        machine.vm.hostname = name
        machine.vm.network :private_network, ip: ip
        machine.vm.provider "virtualbox" do | v |
            v.name = name
            v.customize ["modifyvm", :id, "--memory", 512]

        config.ssh.insert_key = false
        config.vm.provision "file", source: "~/.ssh/vagrant.pub", destination: "~/.ssh/authorized_keys"
        #config.vm.provision "file", source: "~/.ssh/id_rsa.pub", destination: "~/.ssh/authorized_keys"
  
        end
      end
    end
  
  end
  
  
  
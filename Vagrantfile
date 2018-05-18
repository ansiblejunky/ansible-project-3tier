Vagrant.configure("2") do |config|
  
    # image/box
    config.vm.box = "rhel75"

    # authentication
    config.ssh.insert_key = false
    #config.vm.provision "file", source: "~/.ssh/vagrant.pub", destination: "~/.ssh/authorized_keys"
    config.vm.provision "file", source: "~/.ssh/id_rsa.pub", destination: "~/.ssh/authorized_keys"

    # machine defaults
    config.vm.provider "virtualbox" do |vb|
      vb.cpus = "1"
      vb.memory = "512"
    end

    # vagrant-registration => auto register RHEL with subscription manager
    if Vagrant.has_plugin?('vagrant-registration')
        config.registration.username = ENV['SUB_USERNAME']
        config.registration.password = ENV['SUB_PASSWORD']
    end

    # vagrant-hostmanager => manages /etc/hosts entries on host and guest machines
    if Vagrant.has_plugin?('vagrant-hostmanager')
      msg = "vagrant-hostmanager plugin is managing the /etc/hosts entries on your host and guest machines"
      config.hostmanager.enabled = true
      config.hostmanager.manage_host = true
      config.hostmanager.manage_guest = true
      config.hostmanager.ignore_private_ip = false
      config.hostmanager.include_offline = false
    else
      msg = "you need to manually add DNS entries on your host and guest machines"
    end

    # guest machines
    guests = {
      "web1" => "192.168.33.11",
      "web2" => "192.168.33.12",
      "app1" => "192.168.33.13",
      "db1" => "192.168.33.14",
      "db2" => "192.168.33.15"
    }

    # known_hosts - add ssh configuration to your ~/.ssh/config to avoid known_hosts issue
    # VAGRANT
    # Host 192.168.*.*
    #     StrictHostKeyChecking no
    #     UserKnownHostsFile /dev/null
    # Host *.vagrant
    #     StrictHostKeyChecking no
    #     UserKnownHostsFile /dev/null

    guests.each do | name, ip|
      config.vm.define name do | machine |
        machine.vm.hostname = name + ".vagrant"
        machine.vm.network :private_network, ip: ip
        machine.vm.provider "virtualbox" do | v |
            v.name = name
        end

        # remove /etc/hosts entries created by vagrant
        config.vm.provision :shell, :inline => 'sed -i "/127.0.0.1\t$(hostname)/d" /etc/hosts'

        # post-provisioning message
        config.vm.post_up_message = name + "Machine has been provisioned with ... \n\n" +
          "HOSTNAME => " + name + "\n" +
          "IP => " + ip + "\n" +
          "DNS => " + msg

      end
    end
  
  end
  
  
  
Vagrant.configure("2") do |config|

  ###############################################

  # define number of servers
  M = 3

  # provision M VMs as servers
  (1..M).each do |i|

    # name the VMs
    config.vm.define "rke2-server#{i}" do |node|

      # which image to use
      node.vm.box = "opensuse/Leap-15.3.x86_64"

      # sizing of the VMs
      node.vm.provider "libvirt" do |lv|
        lv.memory = 4096
        lv.cpus = 2
      end

      # set the hostname
      node.vm.hostname = "rke2-server#{i}"

      # if this is the last machine
      if i == M

        node.vm.provision "ansible" do |ansible|
          ansible.compatibility_mode = "2.0"
          ansible.limit = "all"
          ansible.groups = {
            "rke2_servers"  => [ "rke2-server1", "rke2-server2", "rke2-server3" ],
          }
          ansible.playbook = "ansible/playbook-vagrant.yml"
        end # node.vm.provision

      end # if-condition last machine

    end # config.vm.define servers

  end # each-loop servers

end # Vagrant.configure

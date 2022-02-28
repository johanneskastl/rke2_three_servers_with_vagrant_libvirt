# rke2_three_servers_with_vagrant_libvirt

Vagrant setup for building a RKE cluster with three controlplane nodes using the vagrant libvirt provider 

This setup creates three controlplane VMs.

Default OS is openSUSE Leap 15.3, but that can be changed in the Vagrantfile. Same holds true for the sizing of the machines.

## Vagrant

1. You need vagrant obviously. And libvirt. And ansible.
2. Fetch the box, per default this is `opensuse/Leap-15.3.x86_64`, using `vagrant box add opensuse/Leap-15.3.x86_64`.
3. Make sure the machine, where you want to run this on, has enough RAM to allow running the VMs. If not, adjust the sizes in the `Vagrantfile`.
4. Run `vagrant up`
5. Run `kubectl --kubeconfig ansible/rke2-kubeconfig get nodes` and you should see your three control-plane nodes.
6. Party!

## Adjusting VM sizes in the Vagrantfile

In case you want or need to change the VM sizing, change the following lines in the `Vagrantfile`:
```
        lv.memory = 4096
        lv.cpus = 2
```

## Increasing or decreasing the number of nodes

In case you want to decrease the number of nodes: Don't. Use [this Vagrant setup](https://github.com/johanneskastl/rke2_singlenode_with_vagrant_libvirt) instead.
The reason is that etcd needs a quorum, i.e. if one node fails the others are still able to make a decision. With three servers it works, but not with two. So the only smaller number of server nodes is one, hence: use the other setup...

In case you want to increase the number of nodes, you need to adjust two lines in the `Vagrantfile`:

Change the number in this line:
```
  # define number of servers
  M = 3
```

Then add the additional hostnames to the list:
```
        ansible.groups = {
          "rke2_servers"  => [ "rke2-server1", "rke2-server2", "rke2-server3" ],
        }
```

## Cleaning up

You can remove your vagrant VMs using `vagrant destroy`, but you need to manually remove the following files before starting again:
```bash
ansible/rke2-kubeconfig
ansible/rke2-token
```

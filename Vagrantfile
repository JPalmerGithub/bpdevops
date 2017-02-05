# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
  config.vm.box = "ubuntu/trusty64"
  config.vm.box_url = "https://cloud-images.ubuntu.com/vagrant/trusty/current/trusty-server-cloudimg-amd64-vagrant-disk1.box"
  config.ssh.keep_alive = true
  config.vagrant.host = :detect

#/////////////////////////////////////////////////

  ##### Ansible Server environment
  config.vm.define :dockerserver, primary:true do |dockerserver|
    dockerserver.vm.network "private_network", :name => 'vboxnet0', :adapter => 2, ip: "192.168.56.155", :netmask => "255.255.255.0", auto_config: false
    dockerserver.vm.network "private_network", ip: "192.168.56.155"
    dockerserver.vm.provider :virtualbox do |virtualbox|
      virtualbox.customize ["modifyvm", :id, "--name", "dockerserver"]
      virtualbox.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
      virtualbox.customize ["modifyvm", :id, "--memory", "3072"]
    end

    dockerserver.ssh.shell = "bash -c 'BASH_ENV=/etc/profile exec bash'"
    dockerserver.vm.provision :shell, path: "init-dockerserver.sh"

    dockerserver.vm.provision "ansible" do |ansible|
        ansible.playbook       = "ansible/provisioning/playbook.yml"
        ansible.inventory_path = "ansible/provisioning/hosts"
        ansible.sudo           = true
    end
  end

#/////////////////////////////////////////////////

end

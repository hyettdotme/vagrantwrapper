# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
  config.vm.box = "trusty64"
  config.vm.box_url = "http://cloud-images.ubuntu.com/vagrant/trusty/current/trusty-server-cloudimg-amd64-vagrant-disk1.box"

  # allows running commands globally in shell for installed composer libraries
  config.vm.provision :shell, path: "vagrant/files/scripts/setup.sh"

  # setup virtual hostname and provision local IP
  config.vm.hostname = "vagrantpress.dev"
  config.vm.network :private_network, :ip => "192.168.50.4"
  config.hostsupdater.aliases = %w{www.vagrantpress.dev}
  config.hostsupdater.remove_on_suspend = true

  config.vm.provision :puppet do |puppet|
    puppet.manifests_path = "vagrant/puppet/manifests"
    puppet.module_path = "vagrant/puppet/modules"
    puppet.manifest_file  = "setup.pp"
  end

  config.vm.provision :puppet do |puppet|
    puppet.manifests_path = "vagrant/puppet/manifests"
    puppet.module_path = "vagrant/puppet/modules"
    puppet.manifest_file  = "init.pp"
    puppet.options="--verbose --debug"
  end
  
  config.vm.provision :puppet do |puppet|
    puppet.manifests_path = "dev/puppet/manifests"
    puppet.module_path = "dev/puppet/modules"
    puppet.manifest_file  = "init.pp"
  end
  
  # Fix for slow external network connections
  config.vm.provider :virtualbox do |vb|
    vb.customize ['modifyvm', :id, '--natdnshostresolver1', 'on']
    vb.customize ['modifyvm', :id, '--natdnsproxy1', 'on']
  end
end

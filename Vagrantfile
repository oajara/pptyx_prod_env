# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure(2) do |config|
#  config.vm.box = "ubuntu/xenial64"
  config.vm.box = "ubuntu/trusty64"
  config.vm.provider "virtualbox" do |v|
#    v.gui = true
    v.memory = 1024
  end
  
  config.vm.define "web" do |web|
    web.vm.network "private_network", ip: "10.100.199.201"
  end

  config.vm.define "db" do |db|
    db.vm.network "private_network", ip: "10.100.199.202"
  end  
  
  if Vagrant.has_plugin?("vagrant-cachier")
    config.cache.scope = :box
  end  
end

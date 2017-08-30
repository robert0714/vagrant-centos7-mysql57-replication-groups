# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure(2) do |config|
  if (/cygwin|mswin|mingw|bccwin|wince|emx/ =~ RUBY_PLATFORM) != nil
    config.vm.synced_folder ".", "/vagrant", mount_options: ["dmode=700,fmode=600"]
  else
    config.vm.synced_folder ".", "/vagrant"
  end
  config.vm.box = "bento/centos-7.3"
  (1..3).each do |i|
    config.vm.define "mysql-0#{i}" do |d|
      d.vm.hostname = "mysql-0#{i}"
      d.vm.network "private_network", ip: "10.100.194.20#{i}", gateway: "10.100.194.1",  auto_config: "false" ,  netmask: "255.255.255.0"
#      d.vm.network "public_network", bridge: "eno4", gateway: "192.168.57.1" , ip: "192.168.57.4#{i}"  ,  netmask: "255.255.255.0" , auto_config: "false"
      d.vm.provider "virtualbox" do |v|
        v.memory = 2048
      end
    end
  end 
  config.vm.define "app" do |d|     
    d.vm.hostname = "app"
#   d.vm.network "public_network", bridge: "eno4", ip: "192.168.57.27", auto_config: "false", netmask: "255.255.255.0" , gateway: "192.168.57.1"
    d.vm.network "private_network", ip: "10.100.194.204", gateway: "10.100.194.1",  auto_config: "false" ,  netmask: "255.255.255.0"
    d.vm.provision :shell , inline: "systemctl restart network"
    d.vm.provision :shell, path: "scripts/bootstrap4CentOs_ansible.sh"
    d.vm.provision :shell, inline: "PYTHONUNBUFFERED=1 ansible-playbook /vagrant/ansible/cd.yml -c local"    
    d.vm.provider "virtualbox" do |v|
      v.memory = 2048
    end
  end   
  if Vagrant.has_plugin?("vagrant-cachier")
    config.cache.scope = :box
  end
  if Vagrant.has_plugin?("vagrant-vbguest")
    config.vbguest.auto_update = false
    config.vbguest.no_install = false
    config.vbguest.no_remote = false
  end
end

# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure(2) do |config|

  config.hostmanager.enabled = true
  config.hostmanager.ignore_private_ip = false
  config.hostmanager.include_offline = true

  config.vm.define "centos" do |centos|
    # centos
    centos.vm.synced_folder ".", "/vagrant", type: "virtualbox"
    centos.vm.box = "centos/7"
    centos.vm.hostname = "centos"
    centos.vm.network "public_network", use_dhcp_assigned_default_route: true
    centos.vm.provision "shell", inline: <<-SHELL
      sudo yum -y update
      sudo yum -y install yum-utils device-mapper-persistent-data lvm2
      sudo yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
      sudo yum-config-manager --enable docker-ce-edge
      sudo yum makecache fast
      sudo yum -y install docker-ce
      sudo yum -y install epel-release
      sudo yum -y install nodejs npm --enablerepo=epel
      sudo mkdir /data
      cd /data
      sudo npm init -y
      sudo npm install express --save
    SHELL
    centos.vm.provision "file", source: "./index.js", destination: "/data/"
  end
end
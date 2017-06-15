# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure(2) do |config|

  config.hostmanager.enabled = true
  config.hostmanager.ignore_private_ip = false
  config.hostmanager.include_offline = true

  config.vm.define "node1" do |node1|
    # centos
    node1.vm.synced_folder ".", "/vagrant", type: "virtualbox"
    node1.vm.box = "centos/7"
    node1.vm.hostname = "node1"
    node1.vm.network "public_network", use_dhcp_assigned_default_route: true
    node1.vm.provision "shell", inline: <<-SHELL
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
      sudo chown vagrant:vagrant -R /data
      sudo npm init -y
      sudo npm install express --save
    SHELL
    node1.vm.provision "file", source: "./index.js", destination: "/data/index.js"
  end

  (2..3).each do |i|
    config.vm.define "node#{i}" do |node|
      node.vm.box = "centos/7"
      node.vm.synced_folder ".", "/vagrant", type: "virtualbox"
      node.vm.hostname = "node#{i}"
      node.vm.network "public_network", use_dhcp_assigned_default_route: true
      node.vm.provision "shell", inline: <<-SHELL
        sudo yum update -y
        sudo yum -y install yum-utils device-mapper-persistent-data lvm2
        sudo yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
        sudo yum-config-manager --enable docker-ce-edge
        sudo yum makecache fast
        sudo yum -y install docker-ce
      SHELL
    end
  end

end
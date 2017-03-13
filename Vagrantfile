# -*- mode: ruby -*-
# vi: set ft=ruby :
Vagrant.require_version ">= 1.8.3"

Vagrant.configure('2') do |config|
  config.vm.box = 'ubuntu/xenial64'
  config.vm.network 'forwarded_port', guest: 80, host: 8080
  config.vm.define :datocansible do |datocansible|
    datocansible.vm.hostname = 'datocansible'
  end

  config.vm.network :private_network, ip: '192.168.60.101'
  config.ssh.forward_agent = true
  config.vm.synced_folder '.', '/vagrant', nfs: true

  config.vm.provider "virtualbox" do |v|
    v.name  = "datocansible"
    host = RbConfig::CONFIG['host_os']
    if host =~ /darwin/
      cpus = `sysctl -n hw.ncpu`.to_i
      mem = `sysctl -n hw.memsize`.to_i / 1024 / 1024 / 4
    elsif host =~ /linux/
      cpus = `nproc`.to_i
      mem = `grep 'MemTotal' /proc/meminfo | sed -e 's/MemTotal://' -e 's/ kB//'`.to_i / 1024 / 4
    else
      cpus = 2
      mem = 1024
    end
    v.customize ['modifyvm', :id, '--memory', mem]
    v.customize ['modifyvm', :id, '--cpus', cpus]
  end

  # Ansible provider
  config.vm.provision :ansible do |ansible|
    ansible.playbook = 'vagrant.yml'
    ansible.host_key_checking = false
  end
end

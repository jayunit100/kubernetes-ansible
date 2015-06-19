# -*- mode: ruby -*-
# vi: set ft=ruby :

# Vagrantfile API/syntax version. Don't touch unless you know what you're doing!
VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|

  centos = 'hansode/fedora-21-server-x86_64'
  script = "provision.sh"
  #discovery_url = `wget https://discovery.etcd.io/new?size=3 -O -`
  discovery_url = "dummy"
  config.hostmanager.enabled = true 

  config.vm.provider "virtualbox" do |v|
     v.memory = 2408
     v.cpus = 1
  end

  #puts "WARNING: Even though we got a discovery URL for now we're not using it"
  #puts discovery_url

  config.vm.define "etcd" do |kube0|
    kube0.vm.box = centos
    kube0.vm.hostname = "kube0.ha"
    kube0.vm.synced_folder ".", "/vagrant"
    kube0.vm.network :private_network, ip: "192.168.4.100"
    #kube0.vm.provision "shell", path:script, args:[discovery_url]
  end

  config.vm.define "kube1" do |kube1|
    kube1.vm.box = centos
    kube1.vm.hostname = "kube1.ha"
    kube1.vm.synced_folder ".", "/vagrant"            
    kube1.vm.network :private_network, ip: "192.168.4.101"
    #kube1.vm.provision "shell", path:script, args:[discovery_url]
  end

  config.vm.define "kube2" do |kube2|
    kube2.vm.box = centos
    kube2.vm.hostname = "kube2.ha"
    kube2.vm.network :private_network, ip: "192.168.4.102"
    kube2.vm.synced_folder ".", "/vagrant"            
    #kube2.vm.provision "shell", path:script, args:[discovery_url]
  end

  ansible.groups = {
     "etcd" => ["kube0"],
  }

  ### First run setup .  This will setup etcd.
  config.vm.provision "ansible" do |ansible|
      ansible.playbook = "setup.yml"
  end
  
  ### Now run flannel, which will use etcd to do its own setup.
  config.vm.provision "ansible" do |ansible|
      ansible.playbook = "flannel.yml"
  end
  
   

  end

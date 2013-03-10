# -*- mode: ruby -*-
# vi: set ft=ruby :

require 'vagrant-ansible'

Vagrant::Config.run do |global_config|
  # All Vagrant configuration is done here. The most common configuration
  # options are documented and commented below. For a complete reference,
  # please see the online documentation at vagrantup.com.

  global_config.vm.box = "precise64"
  global_config.vm.box_url = "http://files.vagrantup.com/precise64.box"

  ## start of controller vm
  global_config.vm.define(:controller) do |config|
    config.vm.network :hostonly, "192.168.206.110"
    config.vm.network :hostonly, "192.168.100.110"
    config.vm.host_name = "controller.localhost"

    config.vm.provision :ansible do |ansible|
      # point Vagrant at the location of your playbook you want to run
      ansible.playbook =  [ "playbooks/setup.yml" ]

      # the Vagrant VM will be put in this host group change this should
      # match the host group in your playbook you want to test
      ansible.hosts = [ "controller" ]
    end

    config.vm.customize [
      "modifyvm", :id,
      "--name", "Controller VM",
      "--memory", "1024"
    ]
  end
  ## end of controller vm


  ## start of network vm
  global_config.vm.define(:network) do |config|
    config.vm.network :hostonly, "192.168.206.120"
    config.vm.network :hostonly, "192.168.100.120"
    config.vm.host_name = "network.localhost"

    config.vm.provision :ansible do |ansible|
      # point Vagrant at the location of your playbook you want to run
      ansible.playbook =  [ "playbooks/setup.yml" ]

      # the Vagrant VM will be put in this host group change this should
      # match the host group in your playbook you want to test
      ansible.hosts = [ "network" ]
    end

    config.vm.customize [
      "modifyvm", :id,
      "--name", "Network VM",
      "--memory", "1024"
    ]
  end
  ## end of network vm


  ## start of compute vms
  computeNodes = {
    :compute00 => {:network0 => "192.168.206.130", :network1 => "192.168.100.130", :nodename => "compute00@192.168.206.130", :host_name => "compute00.localhost"},
    #:compute01 => {:network0 => "192.168.206.131", :network1 => "192.168.100.131", :nodename => "compute00@192.168.206.131", :host_name => "compute01.localhost"},
  }

  computeNodes.each do |name, opts|
    global_config.vm.define name do |compute|
      # uncomment the following line if you want the basebox to start in gui mode
      #compute.vm.boot_mode = :gui
      compute.vm.network :hostonly, opts[:network0]
      compute.vm.network :hostonly, opts[:network1]
      compute.vm.host_name = opts[:host_name]

      compute.vm.provision :ansible do |ansible|
        # point Vagrant at the location of your playbook you want to run
        ansible.playbook =  [ "playbooks/setup.yml" ]

        # the Vagrant VM will be put in this host group change this should
        # match the host group in your playbook you want to test
        ansible.hosts = [ "compute" ]
    end

    compute.vm.customize [
      "modifyvm", :id,
      "--name", opts[:host_name],
      "--memory", "1024",
      "--nicpromisc3", "allow-all"
    ]

    end
  end
  ## end of compute vms


end

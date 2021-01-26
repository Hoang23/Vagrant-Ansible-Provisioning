# -*- mode: ruby -*-
# vi: set ft=ruby :

DIR = File.dirname(__FILE__)

require 'yaml'
$env_config = YAML.load_file(File.join(DIR, 'env.yml'))

#  Configuration setup (there are currently two versions 1 and 2)
Vagrant.configure('2') do |config|

  config.vm.box = 'generic/debian10'
  
  # this is to mount / sync folder from host machine to the virtual machines
  config.vm.synced_folder "./", "/vagrant", type: 'nfs',  disabled: true # necessary to prevent synced folder implementatiion error
 
  # choose ip in range of router dhcp if needing a static ip
  # config.vm.network 'private_network', ip: '192.168.1.51' 
  
  #  Provider (esxi) settings
  config.vm.provider :vmware_esxi do |esxi|

    #  REQUIRED!  ESXi hostname/IP
    esxi.esxi_hostname = $env_config['host_esxi_ip']

    #  ESXi username
    esxi.esxi_username = 'root'

    esxi.esxi_password = 'prompt:'


    esxi.guest_memsize = '2000'

    esxi.guest_numvcpus = '1'

  end

  # can use provisioners such shell furthermore  docker/ansible/chef to execute commands and configure the vm
  config.vm.provision "ansible" do |ansible|
    ansible.playbook = "playbook.yml"
    ansible.extra_vars = { ansible_python_interpreter:"/usr/bin/python3" }
    # ansible.verbose = "vvv" # get more details while provisioning
  end  

end

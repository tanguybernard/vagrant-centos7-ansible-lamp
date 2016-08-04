# -*- mode: ruby -*-
# # vi: set ft=ruby :
 
# Specify minimum Vagrant version and Vagrant API version
Vagrant.require_version ">= 1.6.0"
VAGRANTFILE_API_VERSION = "2"
 
# Require YAML module
require 'yaml'
 
# Read YAML file with box details
servers = YAML.load_file(File.join(File.dirname(__FILE__), 'servers.yml'))

 
# Create boxes
Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
    
    #config.vm.hostname = "vagrantbox"

    config.vm.network :forwarded_port, host: 80, guest: 80, auto_correct: true # website
    config.vm.network :forwarded_port, guest: 443, host: 443, auto_correct: true # ssl
    config.vm.network :forwarded_port, guest: 3306, host: 3306, auto_correct: true # mysql
    config.vm.network :forwarded_port, guest: 9000, host: 9000, auto_correct: true # phpmyadmin
    
    #Avoid, prefer using sftp upload via an IDE like phpstorm
    #config.vm.synced_folder "./", "/var/www/html", type: "rsync", id: "vagrant", :nfs => false,
    #    :mount_options => ["dmode=777", "fmode=666"]
    

    # Iterate through entries in YAML file
    servers.each do |servers|
        config.vm.define servers["name"] do |srv|
            srv.vm.box = servers["box"]
            srv.vm.network "private_network", ip: servers["ip"]
            srv.vm.provider :virtualbox do |vb|
                vb.name = servers["name"]
                vb.memory = servers["ram"]
                #vb.customize ["modifyvm", :id, "--cpus", "1"]
                vb.gui = false
            end
            if servers["prov"] == "ansible"
                srv.vm.provision "ansible" do |ansible|
                    ansible.playbook = "ansible/playbook.yml"
                    ansible.sudo = true
                    #ansible.inventory_path = "playbooks"
                end
            end
        end
    end

    config.vm.provision :shell, inline: "echo Good job, now enjoy your new vbox"

end

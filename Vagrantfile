VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
    config.vm.box = "centos/7"
    config.vm.hostname = "vagrantbox"

    config.vm.network :forwarded_port, host: 80, guest: 80, auto_correct: true # website
    config.vm.network :forwarded_port, guest: 443, host: 443, auto_correct: true # ssl
    config.vm.network :forwarded_port, guest: 3306, host: 3306, auto_correct: true # mysql
    config.vm.network :forwarded_port, guest: 9000, host: 9000, auto_correct: true # phpmyadmin
    config.vm.network :private_network, ip: "10.0.0.10"
    #Avoid, prefer using sftp upload via an IDE like phpstorm
    #config.vm.synced_folder "./", "/var/www/html", type: "rsync", id: "vagrant", :nfs => false,
    #    :mount_options => ["dmode=777", "fmode=666"]

    config.vm.provider "virtualbox" do |vb|
        vb.gui = false
        vb.customize ['modifyvm', :id, '--memory', '1024']
        vb.customize ["modifyvm", :id, "--cpus", "1"]
        vb.customize ["modifyvm", :id, "--nictype1", "virtio"]
    end

    config.vm.provision "ansible" do |ansible|
        ansible.playbook = "ansible/playbook.yml"
        ansible.sudo = true
        #ansible.inventory_path = "playbooks"
    end

    config.vm.provision :shell, inline: "echo Good job, now enjoy your new vbox: http://10.0.0.10"

end

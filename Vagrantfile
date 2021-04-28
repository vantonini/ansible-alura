$script_ansible = <<-SCRIPT
apt-get update && \
apt-get install -y software-properties-common && \
apt-get install -y ansible
apt-get install -y python3-pip
SCRIPT

$script_wordpress = <<-SCRIPT
apt-get update
SCRIPT

Vagrant.configure("2") do |config|
    
  config.vm.box = "ubuntu/bionic64"

  config.vm.provider "virtualbox" do |v|
    v.memory = 1024
  end
  
  # wordpress
  config.vm.define "wordpress" do |wp|
    wp.vm.provider "virtualbox" do |vb|
      vb.name = "ubuntu_focal_wordpress"
    end
    wp.vm.network "public_network", bridge: "Intel(R) Dual Band Wireless-AC 3168", ip: "192.168.0.80"
    
    # adding the public ssh key to our authorized keys
    wp.vm.provision "shell",
      inline: "cat /vagrant/focal/config/id_focal.pub >> .ssh/authorized_keys"
    wp.vm.provision "shell",
      inline: $script_wordpress
  end

  # database
  config.vm.define "mysql" do |db|
    db.vm.provider "virtualbox" do |vb|
      vb.name = "ubuntu_focal_database"
    end
    db.vm.network "public_network", bridge: "Intel(R) Dual Band Wireless-AC 3168", ip: "192.168.0.85"
    
    # adding the public ssh key to our authorized keys
    db.vm.provision "shell",
      inline: "cat /vagrant/focal/config/id_focal.pub >> .ssh/authorized_keys"

  end
  
  # ansible
  config.vm.define "ansible" do |ansible|

    ansible.vm.provider "virtualbox" do |vb|
        vb.name = "ubuntu_focal_ansible"
    end
    ansible.vm.network "public_network", bridge: "Intel(R) Dual Band Wireless-AC 3168", ip: "192.168.0.65" # bridged
    

    ansible.vm.provision "shell",
      inline: $script_ansible
  
    # adding the private ssh key to our authorized_keys
    ansible.vm.provision "shell",
      inline: "cp /vagrant/focal/id_focal /home/vagrant && \
                chmod 600 /home/vagrant/id_focal && \
                chown vagrant:vagrant /home/vagrant/id_focal" 

    # ansible.vm.provision "shell",
    #   inline: "ansible-playbook -i /vagrant/hosts /vagrant/playbook.yml"
  end
  



end
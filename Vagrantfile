# -*- mode: ruby -*-
# vi: set ft=ruby :

# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.
Vagrant.configure("2") do |config|
    # The most common configuration options are documented and commented below.
    # For a complete reference, please see the online documentation at
    # https://docs.vagrantup.com.
    config.vm.define "SIFT"

    # Every Vagrant development environment requires a box. You can search for
    # boxes at https://vagrantcloud.com/search.
    config.vm.box = "ubuntu/xenial64"

	# Set the hostname to something recognizable
	config.vm.hostname = "sift"

    # Disable automatic box update checking. If you disable this, then
    # boxes will only be checked for updates when the user runs
    # `vagrant box outdated`. This is not recommended.
    # config.vm.box_check_update = false

    # Create a forwarded port mapping which allows access to a specific port
    # within the machine from a port on the host machine. In the example below,
    # accessing "localhost:8080" will access port 80 on the guest machine.
    # NOTE: This will enable public access to the opened port
    # config.vm.network "forwarded_port", guest: 80, host: 8080

    # Create a forwarded port mapping which allows access to a specific port
    # within the machine from a port on the host machine and only allow access
    # via 127.0.0.1 to disable public access
    # config.vm.network "forwarded_port", guest: 80, host: 8080, host_ip: "127.0.0.1"

    # Create a private network, which allows host-only access to the machine
    # using a specific IP.
    # config.vm.network "private_network", ip: "192.168.33.10"

    # Create a public network, which generally matched to bridged network.
    # Bridged networks make the machine appear as another physical device on
    # your network.
    # config.vm.network "public_network"

    # Share an additional folder to the guest VM. The first argument is
    # the path on the host to the actual folder. The second argument is
    # the path on the guest to mount the folder. And the optional third
    # argument is a set of non-required options.
    # config.vm.synced_folder "../data", "/vagrant_data"
    config.vm.synced_folder ".", "/vagrant", type: "virtualbox"

	# Persistant storage
	config.persistent_storage.enabled = true
	config.persistent_storage.location = "persistant.vmdk"
	config.persistent_storage.size = 10000
	config.persistent_storage.mountname = 'persistant'
	config.persistent_storage.filesystem = 'ext4'
	config.persistent_storage.mountpoint = '/persistant'
	config.persistent_storage.diskdevice = '/dev/sdc'

    # Provider-specific configuration so you can fine-tune various
    # backing providers for Vagrant. These expose provider-specific options.
    # Example for VirtualBox:
    #
    config.vm.provider "virtualbox" do |vb|
        # Display the VirtualBox GUI when booting the machine
        # vb.gui = true

        # Customize the amount of memory on the VM:
        vb.memory = "4096"

        # Add some more CPUs
        vb.cpus = 4

        # Enable USB 3.0 controller
        vb.customize ["modifyvm", :id, "--usbxhci", "on"]
    end
    #
    # View the documentation for the provider you are using for more
    # information on available options.

    # Enable provisioning with a shell script. Additional provisioners such as
    # Puppet, Chef, Ansible, Salt, and Docker are also available. Please see the
    # documentation for more information about their specific syntax and use.
    config.vm.provision "shell", inline: <<-SHELL
	# download and configure sift installer
    cd /tmp
    curl -Lo /tmp/sift-cli-linux https://github.com/sans-dfir/sift-cli/releases/download/v1.7.1/sift-cli-linux
    curl -Lo /tmp/sift-cli-linux.sha256.asc https://github.com/sans-dfir/sift-cli/releases/download/v1.7.1/sift-cli-linux.sha256.asc
    gpg --keyserver hkp://pgp.mit.edu:80 --recv-keys 22598A94
    gpg --verify sift-cli-linux.sha256.asc
    shasum -a 256 -c sift-cli-linux.sha256.asc
    mv /tmp/sift-cli-linux /usr/local/bin/sift
    chmod 755 /usr/local/bin/sift
	# run sift installer
    /usr/local/bin/sift install --mode=packages-only
    SHELL
end

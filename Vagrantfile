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

  # Every Vagrant development environment requires a box. You can search for
  # boxes at https://vagrantcloud.com/search.
  config.vm.box = "debian/bookworm64"
  config.vm.provision "shell", inline: <<-SHELL
  apt-get update -y
  apt-get upgrade -y
  apt-get install -y bind9 dnsutils
SHELL

#configuracion del master
  config.vm.define "tierra" do |tierra|
    tierra.vm.box = "debian/buster64"
    tierra.vm.hostname = "tierra.sistema.test"
    tierra.vm.network "private_network", ip: "192.168.57.103"
#esta linea manda los files a la carpeta temporal de la maquina, no a la correcta, asi que tendremos que moverlos ahora
    tierra.vm.provision "file", source: "./dnsfiles/named.conf.options", destination: "/tmp/named.conf.options"
    tierra.vm.provision "file", source: "./dnsfiles/named.conf.local.master", destination: "/tmp/named.conf.local"
    tierra.vm.provision "file", source: "./dnsfiles/db.sistema.test", destination: "/tmp/db.sistema.test"
    tierra.vm.provision "file", source: "./dnsfiles/db.57.168.192.in-addr.arpa", destination: "/tmp/db.57.168.192.in-addr.arpa"
#movemos los archivos a donde corresponden
    tierra.vm.provision "shell", inline: <<-SHELL
      apt-get update
      apt-get install -y bind9 bind9utils bind9-doc

      mv /tmp/named.conf.options /etc/bind/named.conf.options
      mv /tmp/named.conf.local /etc/bind/named.conf.local
      mv /tmp/db.sistema.test /etc/bind/db.sistema.test
      mv /tmp/db.57.168.192.in-addr.arpa /etc/bind/db.57.168.192.in-addr.arpa

      systemctl restart bind9
    SHELL
  end

#configuracion del slave
  config.vm.define "venus" do |venus|
    venus.vm.box = "debian/buster64"
    venus.vm.hostname = "venus.sistema.test"
    venus.vm.network "private_network", ip: "192.168.57.102"
#esta linea manda los files a la carpeta temporal de la maquina, no a la correcta, asi que tendremos que moverlos ahora
    venus.vm.provision "file", source: "./dnsfiles/named.conf.options", destination: "/tmp/named.conf.options"
    venus.vm.provision "file", source: "./dnsfiles/named.conf.local.slave", destination: "/tmp/named.conf.local"
#movemos los archivos a donde corresponden
    venus.vm.provision "shell", inline: <<-SHELL
      apt-get update
      apt-get install -y bind9 bind9utils bind9-doc

      mv /tmp/named.conf.options /etc/bind/named.conf.options
      mv /tmp/named.conf.local /etc/bind/named.conf.local

      systemctl restart bind9
    SHELL
  end
#no vamos a crear las que en el trabajo se han puesto explicitamente como imaginarias
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

  # Provider-specific configuration so you can fine-tune various
  # backing providers for Vagrant. These expose provider-specific options.
  # Example for VirtualBox:
  #
  # config.vm.provider "virtualbox" do |vb|
  #   # Display the VirtualBox GUI when booting the machine
  #   vb.gui = true
  #
  #   # Customize the amount of memory on the VM:
  #   vb.memory = "1024"
  # end
  #
  # View the documentation for the provider you are using for more
  # information on available options.

  # Enable provisioning with a shell script. Additional provisioners such as
  # Ansible, Chef, Docker, Puppet and Salt are also available. Please see the
  # documentation for more information about their specific syntax and use.
  # config.vm.provision "shell", inline: <<-SHELL
  #   apt-get update
  #   apt-get install -y apache2
  # SHELL
end

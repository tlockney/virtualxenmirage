# -*- mode: ruby -*-
Vagrant.configure(2) do |config|

  VBOX_VERSION=`VBoxManage | head -1 | cut -d ' ' -f 9`.strip!
  
  config.vm.box = "ubuntu-trusty64"
  config.vm.box_url = "https://cloud-images.ubuntu.com/vagrant/trusty/current/trusty-server-cloudimg-amd64-vagrant-disk1.box"

  config.vm.provider "virtualbox" do |vb|
    # Customize the amount of memory on the VM:
    vb.memory = "4096"
  end

  config.vm.network "private_network", type: "dhcp"

  config.vm.synced_folder ".", "/vagrant", disabled: true

  config.vm.provision "shell", inline: <<-SHELL
    sudo apt-get -y update
    sudo apt-get install -y linux-headers-$(uname -r) linux-headers-generic xen-hypervisor-4.4-amd64 bridge-utils build-essential git xserver-xorg xserver-xorg-core
  SHELL
  config.vm.provision :reload
  config.vm.provision "shell", inline: <<-SHELL
    wget http://download.virtualbox.org/virtualbox/#{VBOX_VERSION}/VBoxGuestAdditions_#{VBOX_VERSION}.iso
    sudo mkdir /media/VBoxGuestAdditions
    sudo mount -o loop,ro VBoxGuestAdditions_#{VBOX_VERSION}.iso /media/VBoxGuestAdditions
    yes | sudo sh /media/VBoxGuestAdditions/VBoxLinuxAdditions.run --nox11
    rm VBoxGuestAdditions_#{VBOX_VERSION}.iso
    sudo umount /media/VBoxGuestAdditions
    sudo rmdir /media/VBoxGuestAdditions

    sudo apt-get -y remove linux-headers-$(uname -r) linux-headers-generic build-essential xserver-xorg xserver-xorg-core
    sudo apt-get -y purge linux-headers-$(uname -r) linux-headers-generic build-essential xserver-xorg xserver-xorg-core
    sudo apt-get -y autoremove

    sudo apt-get -y install avahi-daemon ocaml-compiler-libs ocaml-interp ocaml-base-nox ocaml-base ocaml ocaml-nox ocaml-native-compilers camlp4 camlp4-extra m4 zeroinstall-injector libssl-dev pkg-config
    sudo -u vagrant 0install add opam http://tools.ocaml.org/opam.xml
    sudo -u vagrant opam init -a
    sudo -u vagrant opam install mirage -v -y
  SHELL
  config.vm.provision :reload

end
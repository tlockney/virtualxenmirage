# -*- mode: ruby -*-
Vagrant.configure(2) do |config|

  VBOX_VERSION=`VBoxManage | head -1 | cut -d ' ' -f 9`.strip!
  
  config.vm.box = "ubuntu-trusty64"
  config.vm.box_url = "https://cloud-images.ubuntu.com/vagrant/trusty/current/trusty-server-cloudimg-amd64-vagrant-disk1.box"

  config.vm.provider "virtualbox" do |vb|
    # Customize the amount of memory on the VM:
    vb.memory = "4096"
  end

  config.vm.network "private_network", ip: "192.168.50.4", auto_config: false, adapter: "2"
  # config.vm.network "private_network", type: "dhcp"

  config.vm.synced_folder ".", "/vagrant", disabled: true

  config.vm.provision "shell", inline: <<-SHELL
    apt-get -y update
    apt-get install -y linux-headers-$(uname -r) linux-headers-generic xen-hypervisor-4.4-amd64 bridge-utils build-essential git xserver-xorg xserver-xorg-core
  SHELL
  config.vm.provision :reload
  config.vm.provision "shell", inline: <<-SHELL
    wget http://download.virtualbox.org/virtualbox/#{VBOX_VERSION}/VBoxGuestAdditions_#{VBOX_VERSION}.iso
    mkdir /media/VBoxGuestAdditions
    mount -o loop,ro VBoxGuestAdditions_#{VBOX_VERSION}.iso /media/VBoxGuestAdditions
    yes | sudo sh /media/VBoxGuestAdditions/VBoxLinuxAdditions.run --nox11
    rm VBoxGuestAdditions_#{VBOX_VERSION}.iso
    umount /media/VBoxGuestAdditions
    rmdir /media/VBoxGuestAdditions

    apt-get -y remove linux-headers-$(uname -r) linux-headers-generic build-essential xserver-xorg xserver-xorg-core
    apt-get -y purge linux-headers-$(uname -r) linux-headers-generic build-essential xserver-xorg xserver-xorg-core
    apt-get -y autoremove

    apt-get -y install avahi-daemon ocaml-compiler-libs ocaml-interp ocaml-base-nox ocaml-base ocaml ocaml-nox ocaml-native-compilers camlp4 camlp4-extra m4 zeroinstall-injector libssl-dev pkg-config dnsmasq

    cat > /etc/network/interfaces <<EOF
# /etc/network/interfaces
auto lo
iface lo inet loopback

auto eth0
iface eth0 inet dhcp

auto eth1
iface eth1 inet manual
    pre-up ifconfig $IFACE up
    post-down ifconfig $IFACE down

auto br0
iface br0 inet static
    bridge_ports eth1
    address 192.168.56.5
    broadcast 192.168.56.255
    netmask 255.255.255.0
    # disable ageing (turn bridge into switch)
    up /sbin/brctl setageing br0 0
    # disable stp
    up /sbin/brctl stp br0 off
EOF

    cat > /etc/dnsmasq.conf <<EOF
interface=br0
dhcp-range=192.168.56.150,192.168.56.200,1h
EOF
  SHELL
  
  config.vm.provision "shell", privileged: false, inline: <<-SHELL
    0install add opam http://tools.ocaml.org/opam.xml
    /home/vagrant/bin/opam init -a
    /home/vagrant/bin/opam install mirage -v -y
  SHELL
  config.vm.provision :reload

end

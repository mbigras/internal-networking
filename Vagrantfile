Vagrant.configure("2") do |config|
  config.vm.define "client" do |client|
    client.vm.box = "bento/ubuntu-16.04"
    client.vm.hostname = "client"
    client.vm.network "private_network", virtualbox__intnet: "cats", auto_config: false
    client.vm.provision "shell", inline: <<~SHELL
      rm -f /etc/network/interfaces.d/eth1.cfg
      cat <<EOF > /etc/network/interfaces.d/eth1.cfg
      auto eth1
      iface eth1 inet static
        address 192.168.1.2
        netmask 255.255.255.0
        post-up route add -net 192.168.0.0 netmask 255.255.0.0 gw 192.168.1.1 dev eth1
        pre-down route del -net 192.168.0.0 netmask 255.255.0.0 gw 192.168.1.1 dev eth1
      EOF
      ifdown eth1 && ifup eth1

      echo '192.168.2.2 server' >> /etc/hosts
    SHELL
  end

  config.vm.define "router" do |router|
    router.vm.box = "bento/ubuntu-16.04"
    router.vm.hostname = "router"
    router.vm.network "private_network", ip: "192.168.1.1", virtualbox__intnet: "cats"
    router.vm.network "private_network", ip: "192.168.2.1", virtualbox__intnet: "dogs"
    router.vm.provision "shell", inline: <<~SHELL
      sed -i '/^#net.ipv4.ip_forward=1/s/^#//' /etc/sysctl.conf
      sysctl -p /etc/sysctl.conf
    SHELL
  end

  config.vm.define "server" do |server|
    server.vm.box = "bento/ubuntu-16.04"
    server.vm.hostname = "server"
    server.vm.network "private_network", virtualbox__intnet: "dogs", auto_config: false
    server.vm.provision "shell", inline: <<~SHELL
      rm -f /etc/network/interfaces.d/eth1.cfg
      cat <<EOF > /etc/network/interfaces.d/eth1.cfg
      auto eth1
      iface eth1 inet static
        address 192.168.2.2
        netmask 255.255.255.0
        post-up route add -net 192.168.0.0 netmask 255.255.0.0 gw 192.168.2.1 dev eth1
        pre-down route del -net 192.168.0.0 netmask 255.255.0.0 gw 192.168.2.1 dev eth1
      EOF
      ifdown eth1 && ifup eth1
    SHELL
  end
end

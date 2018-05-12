Vagrant.configure("2") do |config|
  config.vm.define "alice" do |alice|
    alice.vm.box = "bento/ubuntu-16.04"
    alice.vm.hostname = "alice"
    alice.vm.network "private_network", type: "dhcp"
  end
  config.vm.define "bob" do |bob|
    bob.vm.box = "bento/ubuntu-16.04"
    bob.vm.hostname = "bob"
    bob.vm.network "private_network", type: "dhcp"
  end
end

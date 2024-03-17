Vagrant.configure("2") do |config|

  config.vm.provider :libvirt do |virsh|
    virsh.qemu_use_session = false
    virsh.memory = "1536"
  end

  config.vm.define "master1", primary: true do |master1|
    master1.vm.box = "generic/centos9s"
    master1.vm.hostname = "master1"
    master1.vm.network "private_network", ip: "172.29.1.101"
    master1.vm.provision "shell", inline: <<-SHELL
      yum -y update;
    SHELL
  end
  
  config.vm.define "data1" do |data1|
    data1.vm.box = "generic/ubuntu2204"
    data1.vm.hostname = "data1"
    data1.vm.network "private_network", ip: "172.29.1.102"
    data1.vm.provision "shell", inline: <<-SHELL
      apt-get update;
      apt-get full-upgrade -y;
    SHELL
  end

end

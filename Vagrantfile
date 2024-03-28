# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
  config.vm.provider :libvirt do |virsh|
    virsh.qemu_use_session = false
    # global memory in MB
    virsh.memory = "2048"
  end

  config.vm.define "master1" do |master1|
    master1.vm.box = "generic/ubuntu2204"
    master1.vm.box_version = "4.3.12"
    master1.vm.hostname = "master1"
    master1.vm.network "private_network", ip: "172.29.1.101"
    master1.vm.provision "shell", inline: <<-SHELL
      apt-get update;
      apt-get full-upgrade -y;
      sudo apt-get update
      sudo apt-get install -y ca-certificates curl
      sudo install -m 0755 -d /etc/apt/keyrings
      sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
      sudo chmod a+r /etc/apt/keyrings/docker.asc
      echo \
      "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu \
      $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
      sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
      sudo apt-get update
      sudo apt-get install -y docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
      newgrp docker
      sudo adduser vagrant docker
      sudo apt-get install -y jq
      curl -fsSLo /usr/local/bin/kubectl "https://cdn.dl.k8s.io/release/v1.29.3/bin/linux/amd64/kubectl"
      chmod -v +x /usr/local/bin/kubectl
      curl -fsSLo /usr/local/bin/kind https://kind.sigs.k8s.io/dl/v0.22.0/kind-linux-amd64
      chmod -v +x /usr/local/bin/kind
      kind create cluster
      sudo snap install --classic helm --channel="latest/stable"
    SHELL
  end
end

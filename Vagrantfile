# Vagrantfile API/syntax version
VAGRANTFILE_API_VERSION = "2"

# Number of worker nodes to provision
MASTERS = 3

# CPUs for the VMs (2 CPU Cores)
CPU = 2

# Memory for the VMs (2GB)
MEMORY = 2048

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  config.vm.box = "centos/7"
  config.vm.provider "virtualbox" do |vb|
    vb.memory = MEMORY
    vb.cpus = CPU
  end

  # Define Kubernete Master VMs:
  (1..MASTERS).each do |i|
    config.vm.define "k8s-master#{i}" do |master|
      ip = 100 + i
      master.vm.hostname = "k8s-master#{i}.testdomain"
      master.vm.network "private_network", ip: "192.168.99.#{ip}"
      master.vm.provision :shell, inline: <<-SHELL
        echo "Kubernetes Master-#{i}" > /tmp/result
      SHELL
    end
  end
end

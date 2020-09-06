# Vagrantfile API/syntax version
VAGRANTFILE_API_VERSION = "2"

# Number of each type of node to provision
MASTERS = 1
WORKERS = 2

# Resources for Master Nodes
MASTER_CPUS = 2
MASTER_MEMORY = 2048

# Resources for Worker Nodes
WORKER_CPUS = 2
WORKER_MEMORY = 4096


Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  config.vm.box = "centos/7"

  # Define Kubernete Master VMs:
  (1..MASTERS).each do |i|
    config.vm.define :"k8s-master#{i}" do |master|

      master.vm.provider "virtualbox" do |vb|
        vb.cpus = MASTER_CPUS
        vb.memory = MASTER_MEMORY
      end

      ip = 100 + i
      master.vm.hostname = "k8s-master#{i}.localdomain"
      master.vm.network "private_network", ip: "192.168.99.#{ip}"

      master.vm.provision :shell, inline: <<-SHELL
        echo "Kubernetes Master-#{i}" > /tmp/result
      SHELL

      master.vm.provision :ansible do |ansible|
        ansible.verbose = "v"
        ansible.groups = { 
          "master" => ["k8s-master[1:99]"],
          "workers" => ["k8s-worker[1:99]"]
        }
        ansible.host_key_checking = false
        ansible.playbook = "./kube-cluster/kube-dependencies.yml"
      end

      master.vm.provision :ansible do |ansible|
        ansible.verbose = "v"
        ansible.groups = { 
          "master" => ["k8s-master[1:99]"],
          "workers" => ["k8s-worker[1:99]"]
        }
        ansible.host_key_checking = false
        ansible.playbook = "./kube-cluster/master.yml"
      end

    end
  end


  # Define Kubernete Worker VMs:
  (1..WORKERS).each do |i|
    config.vm.define :"k8s-worker#{i}" do |worker|

      worker.vm.provider "virtualbox" do |vb|
        vb.cpus = WORKER_CPUS
        vb.memory = WORKER_MEMORY
      end

      ip = 200 + i
      worker.vm.hostname = "k8s-worker#{i}.localdomain"
      worker.vm.network "private_network", ip: "192.168.99.#{ip}"

      worker.vm.provision :shell, inline: <<-SHELL
        echo "Kubernetes Worker-#{i}" > /tmp/result
      SHELL

      worker.vm.provision :ansible do |ansible|
        ansible.verbose = "v"
        ansible.groups = { 
          "master" => ["k8s-master[1:99]"],
          "workers" => ["k8s-worker[1:99]"]
        }
        ansible.host_key_checking = false
        ansible.playbook = "./kube-cluster/kube-dependencies.yml"
      end

      worker.vm.provision :ansible do |ansible|
        ansible.verbose = "v"
        ansible.groups = { 
          "master" => ["k8s-master[1:99]"],
          "workers" => ["k8s-worker[1:99]"]
        }
        ansible.host_key_checking = false
        ansible.playbook = "./kube-cluster/workers.yml"
        ansible.limit = ["k8s-master1", "k8s-worker#{i}"]
      end

    end
  end

end

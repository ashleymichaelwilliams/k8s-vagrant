# k8s-ansible

This project deploys a Kubernetes cluster within Vagrant VirtualBox machines for testing purposes.


<br>

### Project Notes: 
1. This project was developed and tested using Vagrant and VirtualBox running on a Mac OSX system.
2. Step 2 (optional script below) will not likely work on a Windows OS as it's contains commands specific *nix systems.
3. This project was not designed to deploy a Highly Available (ie. Multi-Master) Kubernetes cluster.


<br>

### Required Prerequisites:
 - Vagrant (pre-installed)
 - VirtualBox (pre-installed)
 - ansible (pre-installed)
 - netaddr (pre-installed)


<br>

## Getting Started:


### Step 1: Provisioning Machines
```
vagrant up
```

<br>

### Step 2: Connecting from Local System (Optional)
```
# Get/Set Vagrant SSH and Port Forwarding Configurations as Variables
VAGRANT_SSH_KEY=$(vagrant ssh-config k8s-master1 | grep -i 'IdentityFile' | awk '{print $2}')
VAGRANT_SSH_PORT=$(vagrant ssh-config k8s-master1 | grep -i 'Port' | awk '{print $2}')
VAGRANT_K8S_API_MASTER_PORT=$(vagrant port k8s-master1 --guest 6443)

# Copy KubeConfig File from Vagrant Machine to Local Machine
scp -i $VAGRANT_SSH_KEY -P $VAGRANT_SSH_PORT -o StrictHostKeyChecking=no vagrant@localhost:/home/vagrant/.kube/config $HOME/.kube/config.vagrant
sed -i '' "s|server: https://.*$|server: https://192.168.99.1:$VAGRANT_K8S_API_MASTER_PORT|" $HOME/.kube/config.vagrant

# List Kubernetes Nodes (Testing)
kubectl get nodes --kubeconfig=$HOME/.kube/config.vagrant
export KUBECONFIG=$KUBECONFIG:$HOME/.kube/config.vagrant
kubectl get nodes
```


<br>

### Step 3: Decommission Machines
```
vagrant destroy -f
```

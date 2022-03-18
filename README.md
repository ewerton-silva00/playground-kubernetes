# Playground Kubernetes

Run Kubernetes locally using Vagrant and VirtualBox

## Requirements

- [`VirtualBox`](https://www.virtualbox.org/) >= 6.1
- [`Vagrant`](https://www.vagrantup.com/) >= 2.2.19
- [`Pip`](https://pypi.org/project/pip/) >= 20.0

## How to use this project

Install the Vagrant's plugins bellow:
```bash
vagrant plugin install vagrant-hosts
vagrant plugin install vagrant-vbguest
```

Install pip package:

**Ubuntu**
```bash
sudo apt install python3-pip -y
```

Clone this repository.
```bash
git clone https://github.com/ewerton-silva00/playground-kubernetes.git
```

Initialize the Python Virtual Environment.
```bash
python -m venv playground-kubernetes
source playground-kubernetes/bin/activate
```

Install Ansible using pip.
```bash
cd playground-kubernetes
pip3 install -r requirements.txt
```

Initialize the Vagrant Box.
```bash
vagrant up
```

Run the playbook file to install Kubernetes cluster.
```bash
ansible-playbook playbook.yaml --extra-vars "containerd_data_dir=/mnt/containerd"
```

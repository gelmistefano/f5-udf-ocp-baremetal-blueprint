# UDF Blueprint - Deploy new from scratch

## UDF Deployment Machines Configuration

| VM UDF Name    | Template                | Capacity                   | Mgmt Address | External Address | Internal Address |
| -------------- | ----------------------- | -------------------------- | ------------ | ---------------- | ---------------- |
| infra-server   | Ubuntu 20.04 LTS Server | 4 vCPU, 16GB RAM,100GB vHD | 10.1.1.4     |                  | 10.1.20.4        |
| bigip-cis      | BIGIP 16.0.1.1-0.0.6    | 4 vCPU, 16GB RAM,105GB vHD | 10.1.1.5     | 10.1.10.5        | 10.1.20.5        |
| volterra-node  | Volterra                | 4 vCPU, 16GB RAM,100GB vHD | 10.1.1.6     |                  |                  |
| linux-jumphost | Ubuntu 20.04 LTS Server | 2 vCPU, 8GB RAM, 50GB vHD  | 10.1.1.7     | 10.1.10.7        | 10.1.20.7        |
| ocp-web        | Ubuntu 20.04 LTS Server | 4 vCPU, 16GB RAM, 50GB vHD | 10.1.1.8     |                  |                  |
| ocp-bootstrap  | Centos 8                | 4 vCPU, 16GB RAM, 50GB vHD | 10.1.1.9     |                  |                  |
| ocp-master-01  | Centos 8                | 4 vCPU, 16GB RAM, 50GB vHD | 10.1.1.10    |                  |                  |
| ocp-master-02  | Centos 8                | 4 vCPU, 16GB RAM, 50GB vHD | 10.1.1.11    |                  |                  |
| ocp-master-03  | Centos 8                | 4 vCPU, 16GB RAM, 50GB vHD | 10.1.1.12    |                  |                  |
| ocp-worker-01  | Centos 8                | 4 vCPU, 16GB RAM, 50GB vHD | 10.1.1.13    |                  |                  |
| ocp-worker-02  | Centos 8                | 4 vCPU, 16GB RAM, 50GB vHD | 10.1.1.14    |                  |                  |
| ocp-worker-03  | Centos 8                | 4 vCPU, 16GB RAM, 50GB vHD | 10.1.1.15    |                  |                  |

## SSH Keys

To allow access to all nodes via ssh from the Ansible host (infra-server) it is necessary to create an SSH key and add it to the authorized keys of all the hosts involved.

- On infra-server

```bash
ssh-keygen -q -t rsa -N '' -f ~/.ssh/id_rsa -b 4096 -C "infra-server" <<<y >/dev/null 2>&1
cat ~/.ssh/id_rsa.pub | tee -a ~/.ssh/authorized_keys
cat ~/.ssh/authorized_keys | sudo tee /root/.ssh/authorized_keys
```

- Copy the `authorized_keys` file from `infra-server`

```bash
cat ~/.ssh/authorized_keys
```

- Log into SSH on all servers and paste the content you copied in the previous step

```bash
echo -e "<PASTE_HERE>" | tee ~/.ssh/authorized_keys
echo -e "<PASTE_HERE>" | sudo tee /root/.ssh/authorized_keys
```

## Infra-server Configuration

Install Ansible and dependencies

```bash
sudo apt update && sudo apt install -y python3 python-is-python3 python3-pip

pip install ansible
```

## Centos Hosts

In order to run Ansible playbooks correctly you need to have python3 installed on the machines.
By default, it is pre-installed on Ubuntu machines while you need to install it via packet manager on Centos machines

```bash
cd /etc/yum.repos.d/
sudo sed -i 's/mirrorlist/#mirrorlist/g' /etc/yum.repos.d/CentOS-*
sudo sed -i 's|#baseurl=http://mirror.centos.org|baseurl=http://vault.centos.org|g' /etc/yum.repos.d/CentOS-*

sudo yum update -y

cd /etc/yum.repos.d/
sudo sed -i 's/mirrorlist/#mirrorlist/g' /etc/yum.repos.d/CentOS-*
sudo sed -i 's|#baseurl=http://mirror.centos.org|baseurl=http://vault.centos.org|g' /etc/yum.repos.d/CentOS-*

sudo yum install -y python38
```

## Copy repository and execute ansible script from infra-server

All the preliminary steps have been completed, now you need to copy the repository to the infra-server machine and run the playbook ansible

```bash
git clone https://github.com/gelmistefano/f5-udf-ocp-baremetal-blueprint.git
cd ansible
ansible-playbook playbook/start.yaml
```

# f5-udf-ocp-baremetal-blueprint

Fork from [this repository](https://github.com/tomminux/f5-udf-ocp-baremetal-blueprint) to automate with Ansible all steps

IMPORTANT NOTE: Tested with OCP 4.9.0 and RHCOS 4.9.0

## Necessary data

Some data are required for the correct deployment of the opeshift cluster

### Select correct timezone

To select the correct time zone to be configured on all hosts, you can run the following command on infra-server host

```bash
timedatectl list-timezones
```

### How to get your pullSecret from redhat

You need to register yourself on the Red Hat Cloud portal and retrieve your pullSecret from

- [user-provisioned-infrastructure](https://cloud.redhat.com/openshift/install/metal/user-provisioned)
- Selecy "Copy pull secret" or "Donwload pull secret"

### OCP Release

You are going to decide which Openshift Container Platform software release you are using.  
Check this [link](https://mirror.openshift.com/pub/openshift-v4/clients/ocp/) and choose your release number

### RHCOS Release

You are going to decide which RedHat Core OS release you are using.  
Check this [link](https://mirror.openshift.com/pub/openshift-v4/dependencies/rhcos/) and choose your release number

### Inventory Hosts personalization

Connect with SSH to infra-server and modify the inventory hosts file with

- Your timezone
- OCP Release version
- RHCOS Main Release version
- RHCOS Full release version
- Your RedHat Pull Secret

Edit the invetory file:

```bash
cd ~/f5-udf-ocp-baremetal-blueprint/ansible
vim inventory/hosts
```

and past there your collected information (below the default settings)

```text
timezone=Europe/Rome
ocp_release=4.9.0
rhcos_branch=4.9
rhcos_release=4.9.0
redhat_pull_secret='Enter the text from your Pull Secrets'
```

### Ansible Playbook to deploy OC Cluster

Connect with SSH to infra-server and execute Ansible playbook

```bash
cd ~/f5-udf-ocp-baremetal-blueprint/ansible
ansible-playbook playbooks/start.yaml
```

At the end of the execution the openshift cluster with the chosen version should be available

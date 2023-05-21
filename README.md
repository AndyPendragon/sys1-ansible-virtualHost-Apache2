# Redhat Ansible
Ansible is an open-source automation tool by which we can automate all cloud provisioning, configuration management, application deployment,intra-service orchestration, and many more IT needs.

It’s the simplest way to automate IT. Ansible is the only automation language that can be used across entire IT teams from systems and network administrators to developers and managers.


## Prerequisites

An Ansible Control Node: The Ansible control node is the machine we’ll use to connect to and control the Ansible hosts over SSH.

## Step 1 — Installing Ansible
To begin using Ansible as a means of managing your server infrastructure, you need to install the Ansible software on the machine that will serve as the Ansible control node.

From your control node, run the following command to include the official project’s PPA (personal package archive) in your system’s list of sources:

```sh
sudo apt-add-repository ppa:ansible/ansible
```

Next, refresh your system’s package index so that it is aware of the packages available in the newly included PPA:

```sh
sudo apt update
```

```sh
sudo apt install ansible
```

Your Ansible control node now has all of the software required to administer your hosts. Next, we will go over how to add your hosts to the control node’s inventory file so that it can control them.

## Step 2 — Setting Up the Inventory File
The inventory file contains information about the hosts you’ll manage with Ansible. You can include anywhere from one to several hundred servers in your inventory file, and hosts can be organized into groups and subgroups. The inventory file is also often used to set variables that will be valid only for specific hosts or groups, in order to be used within playbooks and templates. Some variables can also affect the way a playbook is run, like the ansible_python_interpreter variable that we’ll see in a moment.

To edit the contents of your default Ansible inventory, open the /etc/ansible/hosts file using your text editor of choice, on your Ansible control node:

```sh
sudo nano /etc/ansible/hosts
```

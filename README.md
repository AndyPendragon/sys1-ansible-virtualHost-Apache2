# Ansible by Red Hat

Ansible is an open-source automation tool that simplifies various IT tasks such as cloud provisioning, configuration management, application deployment, and intra-service orchestration. It offers a simple and powerful way to automate IT operations.

With Ansible, you can use a single automation language that caters to different roles within an IT team, including systems administrators, network administrators, developers, and managers.

# Use Ansible to automate the deployment of Apache virtual hosts.

## Prerequisites

To get started, you need to install Ansible. You can follow the tutorial below:

**ansible-tuto**
```sh
git clone https://github.com/leucos/ansible-tuto.git
```
  
## Usage

Follow the instructions below to use this repository:

1. Clone the repository:
```sh
git clone https://github.com/AndyPendragon/sys1-ansible-virtualHost-Apache2.git
```

2. Run the Ansible playbook:
```sh
ansible-playbook deploy_apache.yml -i inventories/production/hosts
```

## Technology

This project is built using Python and utilizes Jinja2 for template rendering with the .j2 extension.

Feel free to modify and customize the playbook and templates according to your specific requirements.

I hope this revision meets your expectations. Let me know if you need further assistance!

## Details

There are 3 important files for Ansible:

**hosts**

The hosts file is an inventory file used by Ansible to specify the target hosts or groups of hosts on which the playbook will be executed. In this case, the [webserver] group is defined with localhost as the host. This means that the playbook will be applied to the local machine where Ansible is being run, simulating the web server environment.

```sh
[webserver]
localhost ansible_connection=local
```

**virtualhost.conf.j2**

The virtualhost.conf.j2 file is a Jinja2 template used to generate the Apache virtual host configuration files. It contains the template code with placeholders ({{ server_name }}, {{ server_alias }}, {{ document_root }}) that will be replaced with the actual values during the playbook execution. This allows for dynamic configuration of the virtual hosts based on the specified variables.

```jinja
<VirtualHost *:80>
    ServerName {{ server_name }}
    ServerAlias www.{{ server_alias }}
    DocumentRoot {{ document_root }}

    # ...
</VirtualHost>
```

**deploy_apache.yml**

The deploy_apache.yml file is the Ansible playbook responsible for deploying the Apache virtual host configuration. It consists of a series of tasks executed in order. The playbook performs tasks such as installing Apache, creating the root directory for virtual hosts, copying the virtual host configuration files from the template, enabling the virtual hosts, and restarting Apache. It uses modules and directives provided by Ansible to interact with the target hosts and perform the necessary configurations.

By combining these files, you can automate the deployment of Apache virtual hosts with custom configurations using Ansible. The hosts file specifies where the playbook will be applied, the virtualhost.conf.j2 file defines the template for virtual host configurations, and the deploy_apache.yml playbook orchestrates the entire deployment process.

```yaml
---
- name: Deploy Apache virtualHost configuration
  hosts: all
  become: true
  tasks:
    - name: Install Apache
      apt:
        name: apache2
        state: present

    - name: Create root directory for virtual hosts
      file:
        path: /var/www/html
        state: directory

    - name: Copy virtual host configuration files
      template:
        src: ../templates/virtualhost.conf.j2
        dest: /etc/apache2/sites-available/{{ item }}.conf
      with_items:
        - api.hei.school
        - back.hei.school
        - front.hei.school
      vars:
        server_name: "{{ item }}"
        server_alias: "www.{{ item }}"
        document_root: "/var/www/html/{{ item }}"

    - name: Enable virtual hosts
      command: a2ensite {{ item }}.conf
      with_items:
        - api.hei.school
        - back.hei.school
        - front.hei.school

    - name: Restart Apache
      service:
        name: apache2
        state: restarted
```
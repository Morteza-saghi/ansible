# Ansible

---

## Introduction to Ansible

Ansible is an open-source automation tool used for configuration management, application deployment, and task automation. It uses a simple, human-readable language called YAML, making it easy to manage systems without requiring deep technical knowledge.

### Key Features of Ansible
- **Agentless**: No need to install any agent on target systems.
- **Idempotent**: Ensures that the system reaches the desired state.
- **Extensible**: Custom modules and plugins can be created.
- **Secure**: Uses OpenSSH for communication, without requiring additional open ports.

## Setting Up Ansible

### Prerequisites
- **Control Node**: The machine where Ansible is installed.
- **Managed Nodes**: The machines managed by Ansible (usually over SSH).

### Installation
Ansible can be installed on most Unix-like systems. Here’s how to install it on a few popular distributions:

#### On Ubuntu

```
sudo apt update
sudo apt install ansible
```

On CentOS/RHEL

```

sudo yum install epel-release
sudo yum install ansible

```

On macOS

```
brew install ansible
```

Verifying Installation


```
ansible --version
```

### Ansible Architecture

#### Ansible's architecture is simple and consists of the following components:

- **Inventory**: Defines the hosts and groups of hosts on which Ansible operates.
- **Modules**: The units of work in Ansible, such as installing packages, managing services, etc.
- **Playbooks**: YAML files containing a series of tasks to be executed on hosts.
- **Plugins**: Extend Ansible’s functionality (e.g., connection, cache, or inventory plugins).
- **Roles**: Group related tasks and configurations into reusable units.
- **Handlers**: Triggered by tasks to run only when needed.
- **Facts**: System information gathered by Ansible when connecting to a host.

### Ansible Inventory

The inventory is a list of managed nodes, defined either as a simple text file (default is /etc/ansible/hosts) or dynamically through a script.
Example Inventory File



```
[webservers]
web1.example.com
web2.example.com

[databases]
db1.example.com
db2.example.com
```


---

### Roles

Roles are a way to group tasks, variables, files, templates, and handlers. They allow you to organize playbooks into reusable components.


#### Creating a Role

```
ansible-galaxy init my_role
```

#### Using a Role in a Playbook

```
---
- hosts: webservers
  roles:
    - my_role
```
---

### Ansible Tasks
tasks are the basic units of work that define actions to be performed on managed nodes.In Ansible, tasks are the basic units of work that define actions to be performed on managed nodes. Each task in Ansible corresponds to a single action or step in a playbook, such as installing a package, copying a file, or restarting a service. Tasks are executed in the order they are listed in a playbook, and Ansible ensures that each task is idempotent, meaning that it can be run multiple times without changing the system beyond its desired state.



a task in an Ansible playbook:
```
---
- hosts: webservers
  tasks:
    - name: Install Apache HTTP Server
      yum:
        name: httpd
        state: present

    - name: Start Apache Service
      service:
        name: httpd
        state: started
```


----


### Handlers

Handlers are tasks that are triggered by other tasks when there is a change:

```
---
- hosts: webservers
  tasks:
    - name: Update Apache config
      template:
        src: /path/to/httpd.conf.j2
        dest: /etc/httpd/conf/httpd.conf
      notify:
        - Restart Apache

  handlers:
    - name: Restart Apache
      service:
        name: httpd
        state: restarted
```
---

### Modules in Ansible
Modules are the workhorses of Ansible. They perform specific tasks like managing packages, services, files, and more.

Commonly Used Modules

- **package**: Manage packages
- **service**: Manage services
- **file**: Manage files and directories
- **copy**: Copy files to remote machines
- **template**: Manage file templates
- **command**: Run commands on remote hosts
- **shell**: Execute shell commands on remote hosts

#### Custom Modules

You can write your own modules in any language (e.g., Python, Bash).




---

### Plugins
Pieces of code that expand Ansible’s core capabilities. Plugins can control how you connect to a managed node (connection plugins), manipulate data (filter plugins) and even control what is displayed in the console (callback plugins).


---

#### Grouping and Variables

You can group hosts and assign variables to them:


```
[webservers]
web1.example.com ansible_user=admin ansible_ssh_private_key_file=/path/to/key
web2.example.com ansible_user=admin

[databases]
db1.example.com ansible_port=2222
db2.example.com ansible_port=2222
```

#### Ansible Configuration

Ansible's behavior can be configured via ansible.cfg. This file can exist in several locations:

```
/etc/ansible/ansible.cfg
~/.ansible.cfg
In the current working directory
```

#### Common Configuration Options

```
[defaults]
inventory = ./inventory
remote_user = ansible
host_key_checking = False
```
---

### Ansible Playbooks
#### Introduction to Playbooks
Playbooks are the heart of Ansible, where you define configurations, deployment steps, and orchestrate tasks.
Playbook Structure

A basic playbook is structured as follows:

```

- hosts: webservers
  tasks:
    - name: Ensure Apache is installed
      yum:
        name: httpd
        state: present

    - name: Start Apache service
      service:
        name: httpd
        state: started
```
---


### Ansible Ad-Hoc Commands
Ad-hoc commands are used for quick tasks without writing playbooks.


Examples

Ping all hosts:

```
ansible all -m ping
```

Install a package:

```
ansible webservers -m yum -a "name=httpd state=present"
```

Restart a service:

```
ansible webservers -m service -a "name=httpd state=restarted"
```


---

#### Conditionals

You can use when to run tasks conditionally:

```
---
- hosts: webservers
  tasks:
    - name: Install Apache on CentOS
      yum:
        name: httpd
        state: present
      when: ansible_facts['os_family'] == "RedHat"
```

---

#### Loops

Loops allow you to repeat tasks for a list of items:

```
---
- hosts: all
  tasks:
    - name: Install multiple packages
      yum:
        name: "{{ item }}"
        state: present
      loop:
        - httpd
        - php
        - mysql-server
```
---
---


#### Working with Ansible Vault

Ansible Vault is used to encrypt sensitive data like passwords, keys, and certificates.
Encrypting a File
```
ansible-vault encrypt secrets.yml

```
#### Decrypting a File

```
ansible-vault decrypt secrets.yml
```

#### Editing an Encrypted File

```
ansible-vault edit secrets.yml
```

#### Running Playbooks with Vault

```
ansible-playbook site.yml --ask-vault-pass
```
---

### Best Practices

- **Use Version Control**: Keep your playbooks in a version control system like Git.
- **Modular Playbooks**: Break down complex playbooks into roles and include them as needed.
- **Idempotence**: Ensure your playbooks can be run multiple times without causing issues.
- **Documentation**: Comment your playbooks and roles.

### Debugging

Use the -vvv flag for more verbose output when running playbooks:

```
ansible-playbook site.yml -vvv
```


### Advanced Ansible Concepts
#### Ansible Tower / AWX
A web-based interface for managing Ansible, with features like role-based access control, job scheduling, and real-time output logging.
Dynamic Inventory

- Use dynamic inventory scripts to pull host information from cloud providers, LDAP, or other sources.
Ansible Pull

- An alternative mode where managed nodes pull configurations from a centralized repository.
Callback Plugins

- Extend Ansible by using callback plugins to integrate with other systems or customize output.

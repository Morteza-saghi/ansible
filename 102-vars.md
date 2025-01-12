## Variables in Ansible
variables," are a core component of Ansible, an open-source automation tool used for configuration management, application deployment, and task automation. Variables in Ansible allow you to define and manage dynamic values, making your playbooks more reusable and flexible.
Here's a quick overview:


---


### Types of Variables
*Playbook Variables*: Defined directly within a playbook.
*Inventory Variables*: Set in the inventory file, often specific to hosts or groups.
*Command-line Variables*: Passed when executing an Ansible command.
*Fact Variables*: Gathered by Ansible from remote systems during execution.


---


### Defining Variables
Variables can be defined in several ways:

#### 1.Playbook: Defined in the vars section of a playbook.

```
- name: Example Playbook
  hosts: all
  vars:
    example_variable: "Hello, World!"
  tasks:
    - name: Print Variable
      debug:
        msg: "{{ example_variable }}"
```


---


#### 2.Inventory File: Set in the inventory file.

```
[webservers]
web1.example.com ansible_user=deploy
web2.example.com ansible_user=deploy
```


---


#### Global: variables that are available to all hosts and playbooks within your Ansible project.These variables can be defined in the vars directory within a role or in a separate file that you include in your playbooks


```
# roles/my_role/vars/main.yml
---
global_var1: "value1"
global_var2: "value2"
```


also we can do it in a Separate File

```
# vars/global_vars.yml
---
global_var1: "value1"
global_var2: "value2"

```

here is how to Include this file in your playbook:

```
# your_playbook.yml
---
- hosts: all
  vars_files:
    - vars/global_vars.yml
  tasks:
    - name: Print global variables
      debug:
        msg: 
          - "global_var1: {{ global_var1 }}"
          - "global_var2: {{ global_var2 }}"
```

---


### Using Variables
Variables are referenced in playbooks and templates using Jinja2 syntax, enclosed in double curly braces: `{{ variable_name }}`.

#### Variable Precedence
Ansible has a specific order of precedence for variables, from least to most influential:

```
Role defaults

Inventory file or script group vars

Inventory file or script host vars

Playbook group vars

Playbook host vars

Host facts

Play vars

Play vars_prompt

Play vars_files

Registered vars

Set_facts / Host facts

Role and include vars

Block vars

Task vars (only for the task)

Extra vars (always win precedence)
```


---


### Best Practices
Use meaningful variable names.

Avoid hardcoding values; use variables instead.

Store sensitive data in encrypted files using Ansible Vault.

By leveraging Ansible variables effectively, you can create more adaptable and maintainable automation scripts. If you have any specific questions or scenarios in mind, feel free to ask!

get more into details with some userfull senarios with ansible vars usage

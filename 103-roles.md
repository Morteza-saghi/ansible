# Ansible Roles: Organizing and Reusing Automation Tasks

Ansible roles are a way to organize and reuse automation tasks efficiently. They allow you to break down complex playbooks into reusable components, making them modular and maintainable.

## 1. Why Use Ansible Roles?
- **Modularity**: Break large playbooks into reusable components.
- **Reusability**: Share roles across different projects.
- **Maintainability**: Makes it easier to manage changes.
- **Readability**: A well-structured role improves understanding.

## 2. Ansible Role Directory Structure
When you create a role using `ansible-galaxy init <role_name>`, it generates the following structure:

```
my_role/
│── defaults/
│   └── main.yml     # Default variables
│── files/
│   └── my_file.conf # Static files to copy
│── handlers/
│   └── main.yml     # Handlers for service restart, reload, etc.
│── meta/
│   └── main.yml     # Role metadata, dependencies, author, license
│── tasks/
│   └── main.yml     # The main list of tasks
│── templates/
│   └── my_template.j2 # Jinja2 templates for configuration files
│── vars/
│   └── main.yml     # Role-specific variables (higher precedence than defaults)
└── README.md        # Documentation for the role
```


## 3. Key Components of a Role
- **tasks/** → Defines automation steps using YAML.
- **handlers/** → Contains handlers for actions like service restarts.
- **templates/** → Stores Jinja2 templates for dynamic configurations.
- **files/** → Stores static files to be copied as-is.
- **vars/** → Defines variables specific to the role.
- **defaults/** → Defines default values for variables.
- **meta/** → Contains metadata like dependencies and author details.

## 4. Example: Creating a Role to Install and Configure Nginx

### Step 1: Create the Role

```
ansible-galaxy init nginx_role
```

Step 2: Define Tasks (nginx_role/tasks/main.yml)

```
---
- name: Install Nginx
  ansible.builtin.apt:
    name: nginx
    state: present
  notify: restart nginx

- name: Copy Nginx Config
  ansible.builtin.template:
    src: nginx.conf.j2
    dest: /etc/nginx/nginx.conf
  notify: restart nginx

- name: Start Nginx
  ansible.builtin.service:
    name: nginx
    state: started
    enabled: yes
```


### Step 3: Define Handlers (nginx_role/handlers/main.yml)

```
---
- name: restart nginx
  ansible.builtin.service:
    name: nginx
    state: restarted
```

### Step 4: Define a Template (nginx_role/templates/nginx.conf.j2)

```
worker_processes auto;
events {
    worker_connections 1024;
}
http {
    server {
        listen 80;
        server_name {{ server_name }};
        root /var/www/html;
    }
}
```


### Step 5: Define Variables (nginx_role/defaults/main.yml)

```
---
server_name: example.com
```


### Step 6: Use the Role in a Playbook
Create a playbook (site.yml) to use the role:

```
---
- hosts: web_servers
  become: yes
  roles:
    - nginx_role
```

### Step 7: Run the Playbook


```
ansible-playbook -i inventory site.yml
```

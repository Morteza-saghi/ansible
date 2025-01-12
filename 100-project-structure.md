### Simple Directory Structure:

```

your_ansible_project/
├── ansible.cfg
├── inventory/
│   └── hosts
├── group_vars/
│   └── all.yml
├── host_vars/
│   └── server1.yml
├── roles/
│   └── common/
│       ├── tasks/
│       │   └── main.yml
│       └── templates/
├── playbooks/
│   ├── site.yml
│   └── webserver.yml
└── README.md

```

### Explanation of Each Directory/File:

**ansible.cfg**: Configuration file for Ansible settings.

**inventory/**: Contains the inventory file.

**hosts**: Defines the list of hosts and groups.

**group_vars/**: Stores group-specific variables.

**all.yml**: Variables applicable to all hosts.

**host_vars/**: Stores host-specific variables.

**server1.yml**: Variables specific to server1.

**roles/**: Directory for roles.

**common/**: Example role for common tasks.

**tasks/main.yml**: Defines tasks to be executed.

**templates/**: Stores template files.

**playbooks/**: Stores playbooks.

**site.yml**: Main playbook.

**webserver.yml**: Playbook specific to web servers.

**README.md**: Documentation for the project.

## Copy module
The Ansible copy module is used to copy files from the local machine to a remote machine. Here’s a basic guide on how to use it:

### 1. Create a Playbook
A playbook is a YAML file that defines tasks to be executed on remote servers. Here's a simple example:

```
---
- name: Copy file to remote server
  hosts: all
  tasks:
    - name: Copy a file
      copy:
        src: /path/to/local/file
        dest: /path/to/remote/destination
        owner: user
        group: group
        mode: '0644'
```

---

### 2. Understand the Parameters

- *src*: The source file on the local machine.

- *dest*: The destination path on the remote machine.

- *owner*: (Optional) The owner of the file on the remote machine.

- *group*: (Optional) The group of the file on the remote machine.

- *mode*: (Optional) The permissions for the file.

### 3. Run the Playbook
Use the ansible-playbook command to execute your playbook:

```
ansible-playbook playbook.yml
```


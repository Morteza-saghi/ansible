# ansible 
ansible tuturial
its gonna be awsome



By default, Ansible looks for the configuration file in the following locations, in order

1    ./ansible.cfg (in the current working directory)

2   ~/.ansible.cfg (in the user's home directory)


Ansible modules?

units of code that can be used to perform specific tasks or actions within an Ansible playbook or task

Each module is designed to handle a particular type of task


Ansible expects the configuration file to be named "ansible.cfg"



some command that i ran already (after puting the ansible.cfg file where is should have been)

ansible all -m ping  ||  -m     to run a module  || all     to all host that you have  





"--become" and "--ask-become-pass"

The "--become" option allows you to run tasks with (root) privileges.It enables the "become" functionality in Ansible, which allows you to execute commands or tasks elevated.       Become Root

"--ask-become-pass" option is used when you want to provide the password required for privilege escalation interactively instead of writing it directly in the playbook or command.           Ask Me the Passcode for root




some coommmands



Apt module : just like the apt command in linux to install

Ansible all -m apt -a name:tmux 
to use this command we need elevated privileges so we do this instead 

Ansible all -m apt -a name:tmux —become –ask-become-pass























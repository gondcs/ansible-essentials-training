create_user
=========

Create a user and upload an ssh public key for remote authentication

Requirements
------------

No specifi required Ansible
Need a default ssh public key or a specific key needs to be called out in a variable.

Role Variables
--------------

# Define the user you want to create
user_name: default
# Define the user state present or absent
user_state: present
# Define the path to the ssh public key
ssh_key: ~/.ssh/id_rsa.pub

Dependencies
------------

None 

Example Playbook
----------------

---
- hosts: all
  tasks:
    - include_role:
        name: create_user
      vars:
        user_name: ruben 
        ssh_key: ~/.ssh/id_rsa.pub


License
-------

MIT

Author Information
------------------

gdelca5@gmail.com
credits to info@kumul.us

A cli tool called awscli, installed using pip, is out of date on remote servers. Ensure it is updated to the latest version.
Write a playbook /home/thor/playbooks/update_version.yml to update aws cli to the latest version. Use inventory /home/thor/playbooks/inventory  
```
- - -
- hosts: all
  gather_facts: no
  tasks: 
    - name: Update awscli
      pip: 
        name: awscli
        state: present
```
[back](https://github.com/MederD/ansible_certification_prep)

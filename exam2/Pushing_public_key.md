A new developer called "John Doe" is recently hired in your team. He needs to get inside all the remote servers via ssh as root user.    
Write a playbook /home/thor/playbooks/give_ssh_access.yml to give ssh access on all the remote servers by pushing his public key: /home/thor/playbooks/john_doe.pub on all of them.  
List of all servers is listed in the inventory /home/thor/playbooks/inventory  

```
---
- hosts: all
  gather_facts: no
  tasks: 
    - name: Pushing public key
      authorized_key:
        user: root
        state: present
        key: " {{ lookup('file', 'john_doe.pub') }}"
        path: /home/thor/playbooks/john_doe.pub
        manage_dir: no
```

[back](https://github.com/MederD/ansible_certification_prep)  

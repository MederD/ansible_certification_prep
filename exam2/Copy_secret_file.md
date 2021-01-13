Write a playbook to copy the secret file located at /home/thor/playbooks/secret_file.txt to all remote hosts at location: /root/.secret_file.txt  
Your playbook must be located at: /home/thor/playbooks/copy_secrets.yml  
Use inventory file: /home/thor/playbooks/inventory. The secret file is encrypted, please use the vault password from the script /home/thor/playbooks/get_vault_pass.py while you execute the playbook.  
```
---
- hosts: all
  gather_facts: no
  tasks: 
    - name: Copy secret file
      copy:
        src: /home/thor/playbooks/secret_file.txt
        dest: /root/.secret_file.txt
```

```
ansible-playbook -i inventory --vault-password-file /home/thor/playbooks/get_vault_pass.py copy_secrets.yml
```

[back](https://github.com/MederD/ansible_certification_prep)  


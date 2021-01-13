It is a recommended practice to apply the security updates on the system in periodic intervals.  
Write a playbook /home/thor/playbooks/patch_system.yml to ensure servers listed on /home/thor/playbooks/inventory are up to date with periodic security updates.  
In CentOS, ensure the playboook installs and configures the yum-cron package to fit the need.  
Confiure the yum-cron config file: /etc/yum/yum-cron.conf as update_cmd = security, to auto-update security packages and ensure yum-cron service is restarted afterwards.  

```
---
- hosts: all
  gether_facts: no
  tasks:
    - name: Install package
      yum:
        name: yum-cron
        state: present
      become: yes
   
    - name: Change configuration
      shell: echo "update_cmd = security" > /etc/yum/yum.conf

    - name: Services
      service: 
        name: yum-cron
        state: started
        become: yes
```
[back](https://github.com/MederD/ansible_certification_prep)  


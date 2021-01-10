Create a playbook facts.yml under ~/playbooks directory on Ansible controller. In this playbook using blockinfile module create a file facts.txt under /root on node02 host and add below given block in it. You will need to enable facts gathering for this task.  
  
This is Ansible managed node `<hostname-of-host> `  {{ ansible_nodename }}  
IP address of host is `<ip-address-of-host>`        {{ ansible_default_ipv4.address }}  
Its OS family is `<os-family>`                      {{ ansible_os_family }}  
  
After that make a copy of this file as index.html under /usr/share/nginx/html/  

```
- hosts: node02
  gather_facts: yes
  tasks:
    - name: Create a facts.txt with blockinfile
      blockinfile:
        path: /root/facts.txt
        create: yes
        block: |
          This is Ansible managed node "{{ ansible_nodename }}"
          IP address of host is "{{ ansible_default_ipv4.address }}"
          Its OS family is "{{ ansible_os_family }}"
         
    - name: Copy facts.txt as index.html under /usr/share/nginx/html/  
      copy:
        src: /root/facts.txt
        dest: /usr/share/nginx/html/  
        remote_src: yes
        
```
         
 
      

Add an entry for node00 in ~/playbooks/inventory file. IP address of node00 host is 172.20.1.100 and SSH user and password is root and Passw0rd. We have a list of users in ~/playbooks/data/users.yml file. There are two groups in there admins and developers which have list of different users. Create a playbook ~/playbooks/add_users.yml to perform below given tasks on node00 node:

a. Add all users given in users.yml on node00.
b. Make sure home directory for all users under developers group is /var/www and for admins it should be default.
c. Set password d3v3l0p3r for all users under developers group and adm$n$ for users under admins group. Make sure to use Ansible vault to encrypt the passwords, use ~/playbooks/secrets/vault.txt file as vault secret file.
d. All users under admins group must be added as sudo user, for that simply make them member of wheel group on node00

Solution:  
cd to /secrets, then:  
```
ansible-vault encrypt_string --vault-password-file vault.txt 'adm$n$' --name 'admin_pass'
ansible-vault encrypt_string --vault-password-file vault.txt 'd3v3l0p3r' --name 'developer_pass'
```
```
- hosts: node00
  vars: 
    admin_pass: !vault |
      *hashed_password*
    developer_pass: !vault |
      *hashed_password*
  tasks:
    - name: use users.yml
      include_vars:
        file: ~/playbooks/data/users.yml
    - name: add groups
      group:
        name: "{{ item }}"
        state: present
      with_items:
        - admins
        - developers

    - name: adding admins
      user:
        name: "{{ item }}"
        groups: admins, wheel
        password: "{{ admin_pass | string | password_hash('sha512') }}"
      become: yes
      become_method: 'sudo'
      with_items: "{{ admins|list }}"
      
    - name: add developers
      user:
        name: "{{ item }}"
        group: developers
        password: "{{ developer_pass | string | password_hash('sha512') }}"
        home: /var/www
      with_items: "{{ developers|list }}"
 ```

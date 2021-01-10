We have php, nginx and mariadb installed on node02 and have a DB mydb created there. The user to connect to DB is myuser and password is mypassword. Create a playbook ~/playbooks/database.yml to perform below given tasks:  

a. Start nginx and mariadb services.  
b. Delete all default files/directories from nginx document root /usr/share/nginx/html/  
c. Download a zip archive from https://github.com/inderpreetaps/ansible-1100-mock-nginx/raw/master/index.php.zip and extract it in /usr/share/nginx/html/  
d. The archive have an index.php file to check DB connectivity. Replace some required DB details in the file using replace or lineinfile module:  
$database = "database"; to $database = "mydb";  
$username = "user"; to $username = "myuser";  
$password = "password"; to $password = "mypassword";  
e. Restart nginx after making required changes.  
```
- hosts: node02
  tasks:
    - name: start service
      service:
         name: "{{ item }}"
         state:  started
         enabled: yes
      with_items:
         - nginx
         - mariadb
         
    - name: Delete all files/ directory
      file:
         path: /usr/share/nginx/html/
         state: absent
         
    - name: Recreate empty directory back
      file: 
         path: /usr/share/nginx/html/
         state: directory
         
    - name: Download zip file
      unarchive:   
         src: https://github.com/inderpreetaps/ansible-1100-mock-nginx/raw/master/index.php.zip
         dest: /usr/share/nginx/html/
         remote_src: yes
         
    - name: change index.php
      replace:
         path: /usr/share/nginx/html/index.php
         regexp: "{{ item.from }}"
         replace: "{{ item.to }}"
      with_items:
         - { from: '\$database.*', to: '$database = "mydb";' }
         - { from: '\$username.*, to: '$username = "myuser";' }
         - { from: '\$password.*', to: '$password = "mypassword";' }
         
    - name: restart nginx
      service:
         name: nginx
         state: restarted
```

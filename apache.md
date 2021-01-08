**Task:**  
1. Install httpd and php on node01  
2. Change apache root directory to /var/www/html/myroot. Make sure directory exists  
3. Copy template to root directory
4. Start httpd service  
5. Open port 80, make it public and permanent

```
- hosts: node01
  tasks:
  - name: Installing packages
    yum:
      name: "{{ item }}"
      state: present
    loop:
      - httpd
      - php
      
  - name: Create directory
    file:
      path: /var/www/html/myroot
      state: directory
      owner: apache
      group: apache
      
  - name: Change httpd config file
    lineinfile:
      path: /etc/httpd/conf/httpd.conf
      regexp: '^DocumentRoot '
      insertafter: '^#DocumentRoot '
      line: DocumentRoot /var/www/html/myroot
   
  - name: Copy template
    copy: 
      src: ~/playbooks/templates/phpinfo.php.j2
      dest: /var/www/html/myroot/phpinfo.php
      owner: apache
      group: apache
   
  - name: Start service
    service:
      name: httpd
      enabled: yes
      state: started
      
  - name: Firewalld rule
    firewalld:
      port: 80/tcp
      state: enabled
      zone: public
      permanent: yes
```

      
      
      
      
      
      
      
      

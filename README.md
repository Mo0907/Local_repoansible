---
- name: Install latest version of packages
  hosts: projectnode2.com
  become: true
  tasks:
    - name: install latest packages on specified node
      ansible.builtin.yum:
        name:
          - httpd
          - firewalld
          - mariadb-server
          - php
          - php-mysqlnd
        state: latest

    - name: firewalld service is enabled and running
      ansible.builtin.service:
        name: firewalld
        enabled: true
        state: started

    - name: firewalld  permits access to http service
      ansible.posix.firewalld:
        service: http
        permanent: true
        state: enabled
        immediate: yes

    - name: httpd is enabled and running
      ansible.builtin.service:
        name: httpd
        enabled: true
        state: started

    - name: mariadb is enabled and running
      ansible.builtin.service:
         name: mariadb
         enabled: true
         state: started


    - name: copy file with owner and permissions
      ansible.builtin.copy:
        src: index.php
        dest: /var/www/html/index.html


"ansiblefile.txt" 48L, 1097B   

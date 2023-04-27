Let us setup password less authentication between Ansible Controller and the web/db servers.

Create a pair of SSH keys for each user (without any passphrase) at /home/thor/.ssh/maria and /home/thor/.ssh/john

And distribute the public keys to the web and database servers - lamp-db and lamp-web.

DB server user is maria and its password is maria. Web server user is john and its password is john.

ssh-keygen -f /home/thor/.ssh/maria and ssh-keygen -f /home/thor/.ssh/john and ssh-copy-id -i /home/thor/.ssh/maria maria@lamp-db and ssh-copy-id -i /home/thor/.ssh/john john@lamp-web

==================================================================================================

Inventory uses private key file for lamp-db

Inventory uses private key file for lamp-web

Inventory no longer contains passwords for lamp-web

Inventory no longer contains passwords for lamp-db

[db_servers]
lamp-db ansible_host=172.20.1.101 ansible_ssh_private_key_file=/home/thor/.ssh/maria ansible_user=maria mysqlservice=mysqld mysql_port=3306 dbname=ecomdb dbuser=ecomuser dbpassword=ecompassword

[web_servers]
lamp-web ansible_host=172.20.1.100 ansible_ssh_private_key_file=/home/thor/.ssh/john ansible_user=john httpd_port=80 repository=https://github.com/kodekloudhub/learning-app-ecommerce.git

==================================================================================================
common dependency installation using playbook
---
- name: Deploy lamp stack application
  hosts: all
  become: yes
  tasks:
    - name: Install common dependencies
      yum:
        name:
          - libselinux-python
          - libsemanage-python
          - firewalld
        state: installed
       
 ==================================================================================================
 
 

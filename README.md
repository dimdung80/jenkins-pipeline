lab-rean
========
If you are running playbook from your CM server or your Local workstation for Wordpress with Nginx/http . 
1. Running from your Local Workstation: 
```
dimdung@devopsvdi wordpress]$ cat hosts 
[ansible]
34.238.82.57
[dimdung@devopsvdi wordpress]$ cat site.yml 
- name: Install WordPress, MariaDB, Nginx, and PHP-FPM
  hosts: ansible
  user: ec2-user
  become: yes
  become_user: root

  roles:
    - common
    - mariadb
    - nginx
    - php-fpm
    - wordpress
[dimdung@devopsvdi wordpress]$ ansible-playbook -i hosts site.yml 
or
[dimdung@devopsvdi wordpress]$ ansible-playbook -i hosts site.yml --private-key=/location_of_your_private_key

$ If you are running Playbook locally : 
[dimdung@devopsvdi wordpress]$ ansible-playbook -i "localhost," -c local site.yml


$ Below is tree of Wordpress Playbook for Single Instances Installation 

[dimdung@devopsvdi tt]$ tree
.
└── lab-rean
    ├── README.md
    └── wordpress
        ├── group_vars
        │   └── all
        ├── hosts
        ├── LICENSE.md
        ├── README.md
        ├── roles
        │   ├── common
        │   │   ├── files
        │   │   │   ├── epel.repo
        │   │   │   ├── nginx.repo
        │   │   │   ├── remi.repo
        │   │   │   ├── RPM-GPG-KEY-EPEL-7
        │   │   │   ├── RPM-GPG-KEY-NGINX
        │   │   │   └── RPM-GPG-KEY-remi
        │   │   └── tasks
        │   │       └── main.yml
        │   ├── mariadb
        │   │   ├── handlers
        │   │   │   └── main.yml
        │   │   ├── tasks
        │   │   │   └── main.yml
        │   │   └── templates
        │   │       └── my.cnf.j2
        │   ├── nginx
        │   │   ├── handlers
        │   │   │   └── main.yml
        │   │   ├── tasks
        │   │   │   └── main.yml
        │   │   └── templates
        │   │       └── default.conf
        │   ├── php-fpm
        │   │   ├── handlers
        │   │   │   └── main.yml
        │   │   ├── tasks
        │   │   │   └── main.yml
        │   │   └── templates
        │   │       └── wordpress.conf
        │   └── wordpress
        │       ├── tasks
        │       │   └── main.yml
        │       └── templates
        │           └── wp-config.php
        └── site.yml

22 directories, 24 files
[dimdung@devopsvdi tt]$ 
```

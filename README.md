REAN Cloud, Technical Assessment
==================================
Here you can find the below details:
1. CloudFormation template for VPC ( 01-rean-vpc.yaml) which will create below resources 
  - VPC with CIDR block 10.108.0.0/16 
  - Public Subnets with Internet Gateway Attached ( 10.108.10.0/24 | 10.108.11.0/24)
  - 2 NAT Gateway 
  - 2 Private Subnets with NAT Gateway Attache to route traffic to Internet for Private Subnets (10.108.20.0/24 | 10.108.21.0/24)
![](images/rean-vpc-samal.PNG)
2. Shared Security Group (02-rean-shared-SecurityGroup.yaml),It can be any Security Group within the VPC but typically Organization used Shared Common services with Tier, which will create below resources. The SG naming convention is. SG-Shared-<Tier>-<Env>-E (E is East Region)
   a. SG-Shared-Elb-Mgmt-E ( All the Shared ELB will used this Security Group)
   b. SG-Shared-Web-Mgmt-E ( All the Shared Web will used this Security Group)
   c. SG-Shared-App-Mgmt-E ( All the Shared App will used this Security Group)
   d. SG-Shared-Dbs-Mgmt-E ( All the Shared Dbs will used this Security Group)
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

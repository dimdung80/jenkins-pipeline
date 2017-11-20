REAN Cloud, Technical Assessment
==================================
Here you can find the below details:
1. CloudFormation template for VPC ( 01-rean-vpc.yaml) which will create below resources 
    - VPC with CIDR block 10.108.0.0/16 
    - 2 Public Subnets with Internet Gateway Attached ( 10.108.10.0/24 | 10.108.11.0/24)
    - 2 NAT Gateway 
    - 2 Private Subnets with NAT Gateway Attache to route traffic to Internet for Private Subnets (10.108.20.0/24 | 10.108.21.0/24)
![](images/rean-vpc-samal.PNG)
2. Shared Security Group (02-rean-shared-SecurityGroup.yaml),It can be any Security Group within the VPC but typically Organization used Shared Common services with Tier, which will create below resources. The SG naming convention is. SG-Shared-<Tier>-<Env>-E (E is East Region)
    - SG-Shared-Elb-Mgmt-E ( All the Shared ELB will used this Security Group)
    - SG-Shared-Web-Mgmt-E ( All the Shared Web will used this Security Group)
    - SG-Shared-App-Mgmt-E ( All the Shared App will used this Security Group)
    - SG-Shared-Dbs-Mgmt-E ( All the Shared Dbs will used this Security Group)
3. WordPress with Playbook (03-rean-ec2-with-userdata-wordpress.yaml) which can be used as Userdata with CloudFormation templates, Jenkins, Ansible Tower or any CI/CD Tools. It will create below resoruces 
    - EC2 instances in existing VPC 
    - Used the Existing Security Group
    - Pull the code from Github Repo (https://github.com/dimdung80/lab-rean.git) and run the playbook locally for this servers 
    - http://ec2-34-229-176-249.compute-1.amazonaws.com 
    - http://ec2-34-229-176-249.compute-1.amazonaws.com/wp-login.php
    ```
          UserData: 
        "Fn::Base64": !Sub |
          #!/bin/bash
          # Install EPEL repo for RHEL 7 
          rpm -Uvh https://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm

          # Install git to clone Github repo 
          yum install git -y 

          # Install Pip packages 
          yum install python-pip -y  

          # Install Ansible after the PIP modules 
          pip install ansible

          #Set clone the git from the Github and run the playbook
          cd /tmp/ 
          git -c http.sslVerify=false clone https://github.com/dimdung80/lab-rean/
          #cd /tmp/lab-rean/workpress/
          #ansible-playbook -i hosts site.yml 
          ansible-playbook -i "localhost," -c local lab-rean/wordpress/site.yml
    ``` 
4. Wordpress with "AWS::CloudFormation::Init:" (04-rean-wordpress.yaml), it will create Wordpress fully Functional CM servers 
    - EC2 instances in existing VPC 
    - Used the Existing Security Group
    - http://ec2-34-203-36-61.compute-1.amazonaws.com/wordpress/ 
    - http://ec2-34-203-36-61.compute-1.amazonaws.com/wordpress/wp-login.php 

5. Playbook-Wordpress 
    ```
        Playbook-wordpress
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

    ``` 
6. Standard Operation Procedure (SOP) docs
    ```
    dimdung@devopsvdi lab-rean]$ tree SOP
SOP
├── 01-REAN Security and Technical Architecture.pdf
├── 02-SOP For Infrastructure CloudFormation (VPC).pdf
├── 03-SOP For Security Group CloudFormation.pdf
├── 04-SOP For Wordpress CloudFormation.pdf
└── 05-SOP For Wordpress with Ansible Playbook + CloudFormation.pdf

0 directories, 5 files
[dimdung@devopsvdi lab-rean]$ 

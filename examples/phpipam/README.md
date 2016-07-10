#PHPIPAM with ANSIBLE
AUTO INSTALL PHPIPAM with ANSIBLE
#1. Playbooks
```sh
phpipam/
├── group_vars
|   └── all
├── roles
|   ├── apache2
|   |   ├── handlers
|   |   |   └── main.yaml
|   |   └── tasks
|   |       └── main.yaml
|   ├── mysql-server
|   |   ├── handlers
|   |   |   └── main.yaml
|   |   └── tasks
|   |   |   └── main.yaml
|   ├── php
|   |   ├── handlers
|   |   |   └── main.yaml
|   |   └── tasks
|   |   |   └── main.yaml
|   ├── phpipam
|   |   ├── tasks
|   |   |   └── main.yaml
|   |   └── templates
|   |   |   └── config.php
| main.yaml
```

#2. How to config
##2.1 Config hosts: `vi phpipam/main.yaml`
```sh
hosts: phpipam
```

##2.2 Config Database: `vi phpipam/group_vars/all`
```sh
#database
ipam_db_host: localhost
ipam_db_user: phpipam
ipam_db_password: phpipam
ipam_db_name: phpipam
ipam_db_port: 3306

#BASE directive
ipam_base: /phpipam/
```

#3. How to run
```sh
apt-get install git
git clone https://github.com/lethanhlinh247/Ansible.git
cd Ansible/examples/phpipam/
ansible-playbook main.yaml
```

#ANSIBLE
##1. Là gì?Dùng để làm gì?
##2. Kiến trúc - Cách hoạt động.
##3. Cài đặt
###3.1 trên Controller
```sh
$ sudo apt-get install software-properties-common
$ sudo apt-add-repository ppa:ansible/ansible
$ sudo apt-get update
$ sudo apt-get install ansible
```

```sh
# install the epel-release RPM if needed on CentOS, RHEL, or Scientific Linux
$ sudo yum install ansible
```
###3.2 Trên remote host.
##4. Configuration file
###4.1 Inventory: `/etc/ansible/hosts`
- Ansible làm việc với nhiều hệ thống trong cùng một thời điểm. Các hệ thống này được cấu hình
trong Ansible’s inventory file, Nơi lưu trữ mặc định là `/etc/ansible/hosts`

- Chúng ta có thể sử dụng nhiều inventory file tại thời điểm và có thể đẩy inventory từ dynamic
hoặc cloud sources, đây là Dynamic Inventory.

- 
```sh
[webservers]
foo.example.com
bar.example.com

[dbservers]
one.example.com
two.example.com
three.example.com
```

Những điều trong ngoặc là tên nhóm, được sử dụng trong việc phân loại các hệ thống và 
quyết định những gì hệ thống bạn đang kiểm soát và cho mục đích gì.

####Inventory Parameters
Note
Ansible 2.0 has deprecated the “ssh” from:
`ansible_ssh_user`, `ansible_ssh_host`, and `ansible_ssh_port` to become 
`ansible_user`, `ansible_host`, and `ansible_port`. 


| Test | Test |
|:-----:|:-----:|
|ansible_connection|Connection type to the host. This can be the name of any of ansible’s connection plugins. SSH protocol types are smart, ssh or paramiko. The default is smart. Non-SSH based types are described in the next section.|
|ansible_host|The name of the host to connect to, if different from the alias you wish to give to it.|
|ansible_port|The ssh port number, if not 22|
|ansible_user|The default ssh user name to use.|
|ansible_ssh_pass|The ssh password to use (this is insecure, we strongly recommend using --ask-pass or SSH keys)|
|ansible_ssh_private_key_file|Private key file used by ssh. Useful if using multiple keys and you don’t want to use SSH agent.|
|ansible_ssh_common_args|This setting is always appended to the default command line for sftp, scp, and ssh. Useful to configure a ProxyCommand for a certain host (or group).|
|ansible_sftp_extra_args|This setting is always appended to the default sftp command line.|
|ansible_scp_extra_args|This setting is always appended to the default scp command line.|
|ansible_ssh_extra_args|This setting is always appended to the default ssh command line.|
|ansible_ssh_pipelining|Determines whether or not to use SSH pipelining. This can override the pipelining setting in `ansible.cfg`.|
|:=:|:=:|

Privilege escalation (see Ansible Privilege Escalation for further details).

|:-----:|:-----:|
|ansible_become|Equivalent to ansible_sudo or ansible_su, allows to force privilege escalation|
|ansible_become_method|Allows to set privilege escalation method|
|ansible_become_user|Equivalent to `ansible_sudo_user` or `ansible_su_user`, allows to set the user you become through privilege escalation|
|ansible_become_pass|Equivalent to `ansible_sudo_pass` or `ansible_su_pass`, allows you to set the privilege escalation password|
|:=:|:=:|

Remote host environment parameters:

|:-----:|:-----:|
|ansible_shell_type|The shell type of the target system. You should not use this setting unless you have set the ansible_shell_executable to a non-Bourne (sh) compatible shell. By default commands are formatted using sh-style syntax. Setting this to csh or fish will cause commands executed on target systems to follow those shell’s syntax instead.|
|ansible_python_interpreter|The target host python path. This is useful for systems with more than one Python or not located at /usr/bin/python such as *BSD, or where /usr/bin/python is not a 2.X series Python. We do not use the /usr/bin/env mechanism as that requires the remote user’s path to be set right and also assumes the python executable is named python, where the executable might be named something like python2.6.|
|ansible_*_interpreter|Works for anything such as ruby or perl and works just like ansible_python_interpreter. This replaces shebang of modules which will run on that host.|
|:=:|:=:|

 New in version 2.1.
 
|:-----:|:-----:|
|ansible_shell_executable|This sets the shell the ansible controller will use on the target machine, overrides executable in ansible.cfg which defaults to /bin/sh. You should really only change it if is not possible to use /bin/sh (i.e. /bin/sh is not installed on the target machine or cannot be run from sudo.).|
|:=:|:=:|


###4.2
```sh
[defaults]
hostfile = hosts
remote_user = vagrant
private_key_file = .vagrant/machines/default/virtualbox/private_key
host_key_checking = False
```
Ansible uses `/etc/ansible/hosts` as the default location for the inventory file.


##5. Module

##6. Playbooks.


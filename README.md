#ANSIBLE
#1. Là gì?Dùng để làm gì?
Configuration management (CM) là công cụ thực hiện việc thay đổi trạng thái hiện tại của hệ thống sang trạng thái được xác định trước. Hay nói cách khác, là công cụ hỗ trợ, cấu hình, cài đặt hệ thống một cách tự động.

Configuration management có rất nhiều công cụ như Ansible, Chef, Puppet, Saltstack ...

Lợi ích của configuration management:
  - Giúp thực hiện công việc triển khai hệ thống đơn giản và thuận tiện.
  - Hạn chế những công đoạn lặp lại, tiết kiệm thời gian
  - Có thể sử dụng lại cho những hệ thống tương tự.
  - Linh hoạt, mềm dẻo trong quản lý.

Hạn chế
  - Nếu không phải triển khai 1 hệ thống đủ lớn, hoặc chỉ phải thực hiện trên 1, 2 server thì thực sự không cần thiết dùng đến CM. Viết ra kịch bản có khi tiêu tốn nhiều thời gian hơn việc bạn thực hiện nó bằng lệnh.

  - Trong những tình huống như hệ thống gặp sự cố hay troubleshooting thì CM không có nhiều tác dụng. Đó là những tình huống cần sự cẩn thận, tránh những sai sót không đáng có.

Ansible đang là công cụ Configuration Management khá nổi bật hiện nay.

Ansible là một công cụ tự động hóa CNTT. Nó có thể cấu hình hệ thống, triển khai phần mềm, và dàn xếp các nhiệm vụ CNTT tiên tiến hơn như triển khai liên tục hoặc không cập nhật thời gian chết.

Ansible là phi tập trung - nó dựa trên các thông tin hệ điều hành hiện tại của bạn để kiểm soát quyền truy cập vào các máy từ xa; nếu cần thiết nó có thể dễ dàng kết nối với Kerberos, LDAP, và các hệ thống quản lý chứng thực tập trung khác.

module Ansible là nguồn lực được phân phối cho các nút từ xa để làm cho họ thực hiện các nhiệm vụ cụ thể hoặc phù hợp với một trạng thái đặc biệt. Ansible sau một "kèm theo pin" triết lý, vì vậy bạn có rất nhiều module tuyệt vời cho tất cả các cách thức của nhiệm vụ CNTT trong việc phân phối lõi. Điều này có nghĩa là mô-đun cũng up-to-date và bạn không cần phải săn cho thực hiện điều đó sẽ làm việc trên nền tảng của bạn. Bạn có thể nghĩ của thư viện module như một hộp công cụ đầy đủ các công cụ quản lý hệ thống hữu ích, và playbooks như các hướng dẫn để xây dựng một cái gì đó sử dụng những công cụ này.


  - Là công cụ mã nguồn mở dùng để quản lý cài đặt, cấu hình hệ thống một cách tập trung và cho phép thực thi câu lệnh điều khiển.
  - Sử dụng SSH (hoặc Powershell) và các module được viết bằng ngôn ngữ Python để điểu khiển hệ thống.
  - Sử dụng định dạng JSON để hiển thị thông tin và sử dụng YAML (Yet Another Markup Language) để xây dựng cấu trúc mô tả hệ thống.

#2. Kiến trúc - Cách hoạt động.

Đặc điểm của Ansible
  - Không cần cài đặt phần mềm lên các agent, chỉ cần cài đặt tại master.
  - Không service, daemon, chỉ thực thi khi được gọi
  - Bảo mật cao (do sử dụng giao thức SSH để kết nối)
  - Cú pháp dễ đọc, dễ học, dễ hiểu

Yêu cầu cài đặt
  - Hệ điều hành: Linux (Redhat, Debian, Ubuntu, Centos, ...), Windows
  - Thư viện Jinja2: dùng để xây dựng template cấu hình
  - Thư viện PyYAML: hỗ trợ cấu trúc YAML
  - Python 2.4 trở lên

Ansible works by connecting to your nodes and pushing out small programs, called "Ansible Modules" to them. These programs are written to be resource models of the desired state of the system. Ansible then executes these modules (over SSH by default), and removes them when finished.

Your library of modules can reside on any machine, and there are no servers, daemons, or databases required. Typically you'll work with your favorite terminal program, a text editor, and probably a version control system to keep track of changes to your content.

SSH KEYS ARE YOUR FRIENDS

Passwords are supported, but SSH keys with ssh-agent are one of the best ways to use Ansible.  Though if you want to use Kerberos, that's good too.  Lots of options! Root logins are not required, you can login as any user, and then su or sudo to any user.

Ansible's "authorized_key" module is a great way to use ansible to control what machines can access what hosts. Other options, like kerberos or identity management systems, can also be used.

#3. Cài đặt
##3.1 trên Controller
- Trên ubuntu
```sh
$ sudo apt-get install software-properties-common
$ sudo apt-add-repository ppa:ansible/ansible
$ sudo apt-get update
$ sudo apt-get install ansible
```
- Trên centos
```sh
$ sudo yum install ansible
```

- Tạo ssh key
```sh
ssh-keygen -t rsa -b 4096
```
- Copy file key sang hosts.
```sh
ssh-copy-id ansible@10.10.10.200
```

##3.2 Trên remote host.
- Tạo tài khoản truy cập SSH.

  Ta nên tạo một tài khoản khác phục vụ cho ansible.

  Tạo tài khoản:
```sh
$ adduser ansible
```

- Cấu hình cho phép tài khoản ansible sử dụng sudo mà không cần mật khẩu.
```sh
$ /vi/etc/sudoers.d/ansible
ansible ALL=(ALL) NOPASSWD:ALL
```

#4. Configuration file
##4.1 Inventory: `/etc/ansible/hosts`
- Ansible làm việc với nhiều hệ thống trong cùng một thời điểm. Các hệ thống này được cấu hình
trong Ansible’s inventory file, Nơi lưu trữ mặc định là `/etc/ansible/hosts`

- Chúng ta có thể sử dụng nhiều inventory file tại thời điểm và có thể đẩy inventory từ dynamic
hoặc cloud sources, đây là Dynamic Inventory.

- File iventory để giúp Ansible biết các server mà nó cần kết nối sử dụng SSH ,
thông tin kết nối nó yêu cầu và các tùy chọn biến gắn liền với các server này.

- File inventory có định dạng là INI. Trong file inventory, chúng ta có thể chỉ định nhiều hơn một máy chủ và gom chúng thành nhiều nhóm.

```sh
[webservers]
foo.example.com
bar.example.com

[dbservers]
one.example.com
two.example.com
three.example.com
```

Trong đó:

- [webservers], [dbservers] là các group.
- foo.example.com là các hostname của các host (có thể sử dụng địa chỉ ip của host).

###4.1.1. Inventory Parameters
- Là các tùy chọn đi kèm với các server, được cấu hình trong file inventory: `/etc/ansible/hosts`

- Note: Ansible 2.0 has deprecated the “ssh” from:
`ansible_ssh_user`, `ansible_ssh_host`, and `ansible_ssh_port` to become
`ansible_user`, `ansible_host`, and `ansible_port`.

| Tên | Ý nghĩa |
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
|ansible_become|Equivalent to ansible_sudo or ansible_su, allows to force privilege escalation|
|ansible_become_method|Allows to set privilege escalation method|
|ansible_become_user|Equivalent to `ansible_sudo_user` or `ansible_su_user`, allows to set the user you become through privilege escalation|
|ansible_become_pass|Equivalent to `ansible_sudo_pass` or `ansible_su_pass`, allows you to set the privilege escalation password|
|ansible_shell_type|The shell type of the target system. You should not use this setting unless you have set the ansible_shell_executable to a non-Bourne (sh) compatible shell. By default commands are formatted using sh-style syntax. Setting this to csh or fish will cause commands executed on target systems to follow those shell’s syntax instead.|
|ansible_python_interpreter|The target host python path. This is useful for systems with more than one Python or not located at /usr/bin/python such as *BSD, or where /usr/bin/python is not a 2.X series Python. We do not use the /usr/bin/env mechanism as that requires the remote user’s path to be set right and also assumes the python executable is named python, where the executable might be named something like python2.6.|
|ansible_*_interpreter|Works for anything such as ruby or perl and works just like ansible_python_interpreter. This replaces shebang of modules which will run on that host.|
|ansible_shell_executable|This sets the shell the ansible controller will use on the target machine, overrides executable in ansible.cfg which defaults to /bin/sh. You should really only change it if is not possible to use /bin/sh (i.e. /bin/sh is not installed on the target machine or cannot be run from sudo.).|

###4.1.2 Iventory Dynamic



##4.2 `ansible.cfg`

ansible.cfg in the current working directory, .ansible.cfg in the home directory or /etc/ansible/ansible.cfg, whichever it finds first
- Nội dung mặc định của file ansible.cfg: https://raw.githubusercontent.com/ansible/ansible/devel/examples/ansible.cfg
- Một số thiết lập trong thẻ [defaults]:

| Tên | Ý nghĩa|
|:---:|:------:|
|action_plugins| Actions are pieces of code in ansible that enable things like module execution, templating, and so forth.`action_plugins = ~/.ansible/plugins/action_plugins/:/usr/share/ansible_plugins/action_plugins`|
|ansible_managed| Ansible-managed is a string that can be inserted into files written by Ansible’s config templating system, if you use a string like: `{{ ansible_managed }}`|
|ask_pass| This controls whether an Ansible playbook should prompt for a password by default. The default behavior is no: `ask_pass=True`|
|ask_sudo_pass|Similar to ask_pass, this controls whether an Ansible playbook should prompt for a sudo password by default when sudoing. The default behavior is also no: `ask_sudo_pass=True`|
|ask_vault_pass|This controls whether an Ansible playbook should prompt for the vault password by default. The default behavior is no:`ask_vault_pass=True`|
|inventory|This is the default location of the inventory file, script, or directory that Ansible will use to determine what hosts it has available to talk to: `inventory = /etc/ansible/hosts`|
|library|This is the default location Ansible looks to find modules:`library = /usr/share/ansible`|
|local_tmp|When Ansible gets ready to send a module to a remote machine it usually has to add a few things to the module: Some boilerplate code, the module’s parameters, and a few constants from the config file. This combination of things gets stored in a temporary file until ansible exits and cleans up after itself. The default location is a subdirectory of the user’s home directory. If you’d like to change that, you can do so by altering this setting:`local_tmp = $HOME/.ansible/tmp`|
|log_path|If present and configured in ansible.cfg, Ansible will log information about executions at the designated location. Be sure the user running Ansible has permissions on the logfile:`log_path=/var/log/ansible.log`|
|private_key_file|If you are using a pem file to authenticate with machines rather than SSH agent or passwords, you can set the default value here to avoid re-specifying `--private-key` with every invocation:`private_key_file=/path/to/file.pem`|
|remote_port|This sets the default SSH port on all of your systems, for systems that didn’t specify an alternative value in inventory. The default is the standard 22:`remote_port = 22`|
|remote_tmp|Ansible works by transferring modules to your remote machines, running them, and then cleaning up after itself. In some cases, you may not wish to use the default location and would like to change the path. You can do so by altering this setting:`remote_tmp = $HOME/.ansible/tmp`|
|remote_user|This is the default username ansible will connect as for /usr/bin/ansible-playbook. Note that /usr/bin/ansible will always default to the current user if this is not defined:`remote_user = root`|
|timeout|This is the default SSH timeout to use on connection attempts:`timeout = 10`|
|sudo_exe|If using an alternative sudo implementation on remote machines, the path to sudo can be replaced here provided the sudo implementation is matching CLI flags with the standard sudo:`sudo_exe=sudo`|
|sudo_user|This is the default user to sudo to if --sudo-user is not specified or ‘sudo_user’ is not specified in an Ansible playbook. The default is the most logical: ‘root’:`sudo_user=root`|
|become|The equivalent of adding sudo: or su: to a play or task, set to true/yes to activate privilege escalation. The default behavior is no:`become=True`|
|become_user|The equivalent to ansible_sudo_user or ansible_su_user, allows to set the user you become through privilege escalation. The default is ‘root’:`become_user=root`|
|become_ask_pass| Ask for privilege escalation password, the default is False:`become_ask_pass=True`|

- Các thiết lập khác thao khảo tại đây: http://docs.ansible.com/ansible/intro_configuration.html


#5. Module
- Modules (also referred to as “task plugins” or “library plugins”) are the ones that do the actual work in ansible, they are what gets executed in each playbook task. But you can also run a single one using the ‘ansible’ command.

- Documentation for each module can be accessed from the command line with the ansible-doc tool:
```sh
ansible-doc yum
```
- A list of all installed modules is also available:
```sh
ansible-doc -l
```
- All modules technically return JSON format data, though if you are using the command line or playbooks, you don’t really need to know much about that. If you’re writing your own module, you care, and this means you do not have to write modules in any particular language – you get to choose.

- Core Modules: These are modules that the core ansible team maintains and will always ship with ansible itself. They will also receive slightly higher priority for all requests than those in the “extras” repos.
- Extras Modules: These modules are currently shipped with Ansible, but might be shipped separately in the future. They are also mostly maintained by the community. Non-core modules are still fully usable, but may receive slightly lower response rates for issues and pull requests.


#6. Ad-Hoc Commands
- An ad-hoc command is something that you might type in to do something really quick, but don’t want to save for later.
##6.1 Shell Commands
The command - Executes a command on a remote node module does not support shell variables and things like piping. If we want to execute a module using a shell, use the ‘shell’ module instead. Read more about the differences on the About Modules page.
```sh
$ ansible raleigh -m shell -a 'echo $TERM'
```

##6.2 File Transfer
- To transfer a file directly to many servers:
```sh
$ ansible atlanta -m copy -a "src=/etc/hosts dest=/tmp/hosts"
```

- The file module allows changing ownership and permissions on files. These same options can be passed directly to the copy module as well:
```sh
$ ansible webservers -m file -a "dest=/srv/foo/a.txt mode=600"
$ ansible webservers -m file -a "dest=/srv/foo/b.txt mode=600 owner=mdehaan group=mdehaan"
```
##6.3 Managing Packages
- Ensure a package is installed, but don’t update it:
```sh
$ ansible webservers -m yum -a "name=acme state=present"
```

- Ensure a package is installed to a specific version:
```sh
$ ansible webservers -m yum -a "name=acme-1.5 state=present"
```

- Ensure a package is at the latest version:
```sh
$ ansible webservers -m yum -a "name=acme state=latest"
```

- Ensure a package is not installed:
```sh
$ ansible webservers -m yum -a "name=acme state=absent"
```

##6.4 Users and Groups
The ‘user’ module allows easy creation and manipulation of existing user accounts, as well as removal of user accounts that may exist:
```sh
$ ansible all -m user -a "name=foo password=<crypted password here>"

$ ansible all -m user -a "name=foo state=absent"
```

##6.5 Deploying From Source Control
Deploy your webapp straight from git:
```sh
$ ansible webservers -m git -a "repo=git://foo.example.org/repo.git dest=/srv/myapp version=HEAD"
```

##6.6 Managing Services
- Ensure a service is started on all webservers:
```sh
$ ansible webservers -m service -a "name=httpd state=started"
```

- Alternatively, restart a service on all webservers:
```sh
$ ansible webservers -m service -a "name=httpd state=restarted"
```

- Ensure a service is stopped:
```sh
$ ansible webservers -m service -a "name=httpd state=stopped"
```

#7. Playbooks.
Simply put, playbooks are the basis for a really simple configuration management and multi-machine deployment system, unlike any that already exist, and one that is very well suited to deploying complex applications.
- Playbooks được viết dưới dạng YAML format
- Có thể có nhiều play trong một playbook
- Mục tiêu của play là để ánh xạ một nhóm các máy chủ với một số vai trò được xác định rõ, thể hiện thông qua các cuộc gọi ansible đến tasks.
- Ví dụ:
```sh
---
- hosts: webservers
  vars:
    http_port: 80
    max_clients: 200
  tasks:
  - name: ensure apache is at the latest version
    yum: pkg=httpd state=latest
  - name: write the apache config file
    template: src=/srv/httpd.j2 dest=/etc/httpd.conf
    notify:
    - restart apache
  - name: ensure apache is running (and enable it at boot)
    service: name=httpd state=started enabled=yes
  handlers:
    - name: restart apache
      service: name=httpd state=restarted
```

Trong đó:
  - `hosts:` xác định đối tượng sẽ thực thi playbook này.
  - `vars:` các biến dùn trong play, trong ví dụ này các biến sẽ được dùng để cấu hình apache
  - `tasks:` liệt kê các task cần thực hiện
  - `name:` tên của task
  - `yum`: template, service: các module sử dụng
  - `notify:` giống như trigger, để gọi đến 1 task khác khi task hiện tại thực hiện thành công.
  - `handlers:` khai báo các task notify

- Chạy playbook:
```sh
$ ansible-playbook webserver.yml
```

- Cấu trúc 1 playbooks chuẩn
```sh
production                # inventory file for production servers  
stage                     # inventory file for stage environment

group_vars/  
   group1                 # here we assign variables to particular groups
   group2                 # ""
host_vars/  
   hostname1              # if systems need specific variables, put them here
   hostname2              # ""

library/                  # if any custom modules, put them here (optional)  
filter_plugins/           # if any custom filter plugins, put them here (optional)

site.yml                  # master playbook  
webservers.yml            # playbook for webserver tier  
dbservers.yml             # playbook for dbserver tier

roles/  
    common/               # this hierarchy represents a "role"
        tasks/            #
            main.yml      #  <-- tasks file can include smaller files if warranted
        handlers/         #
            main.yml      #  <-- handlers file
        templates/        #  <-- files for use with the template resource
            ntp.conf.j2   #  <------- templates end in .j2
        files/            #
            bar.txt       #  <-- files for use with the copy resource
            foo.sh        #  <-- script files for use with the script resource
        vars/             #
            main.yml      #  <-- variables associated with this role
        defaults/         #
            main.yml      #  <-- default lower priority variables for this role
        meta/             #
            main.yml      #  <-- role dependencies

    webtier/              # same kind of structure as "common" was above, done for the webtier role
    monitoring/           # ""
    fooapp/               # ""
```

Trong đó:
  - `production:` giống file /etc/ansible/hosts, liệt kê group, host
  - `group_vars/*:` đặt các biến chung cho cùng 1 nhóm, ví dụ [webservers] có biến listen_port: 80
  - `host_vars/*:` đặt các biến riêng cho từng host
  - `roles/*:` đặt các role, ví dụ các host trong [webservers] gọi đến role webtier

##Tài liệu tham khảo
https://www.ansible.com/how-ansible-works

http://docs.ansible.com/ansible/intro.html

http://blog.vccloud.vn/configuration-management/

http://www.tecmint.com/install-and-configure-ansible-automation-tool-in-linux/

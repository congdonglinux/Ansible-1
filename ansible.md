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
  - Mặc dù rất tốt nhưng CM không phải là vạn năng. Nếu không phải triển khai 1 hệ thống đủ lớn, hoặc chỉ phải thực hiện trên 1, 2 server thì thực sự không cần thiết dùng đến CM. Viết ra kịch bản có khi tiêu tốn nhiều thời gian hơn việc bạn thực hiện nó bằng lệnh.

  - Trong những tình huống như hệ thống gặp sự cố hay troubleshooting thì CM không có nhiều tác dụng. Đó là những tình huống cần sự cẩn thận, tránh những sai sót không đáng có.

Ansible đang là công cụ Configuration Management khá nổi bật hiện nay.  
  - Là công cụ mã nguồn mở dùng để quản lý cài đặt, cấu hình hệ thống một cách tập trung và cho phép thực thi câu lệnh điều khiển.
  - Sử dụng SSH (hoặc Powershell) và các module được viết bằng ngôn ngữ Python để điểu khiển hệ thống.
  - Sử dụng định dạng JSON để hiển thị thông tin và sử dụng YAML (Yet Another Markup Language) để xây dựng cấu trúc mô tả hệ thống.

#2. Kiến trúc - Cách hoạt động.

Đặc điểm của Ansible
  - Không cần cài đặt phần mềm lên các agent, chỉ cần cài đặt tại master.
  - Không service, daemon, chỉ thực thi khi được gọi
  - Bảo mật cao ( do sử dụng giao thức SSH để kết nối )
  - Cú pháp dễ đọc, dễ học, dễ hiểu

Yêu cầu cài đặt
  - Hệ điều hành: Linux (Redhat, Debian, Ubuntu, Centos, ...), Windows
  - Thư viện Jinja2: dùng để xây dựng template cấu hình
  - Thư viện PyYAML: hỗ trợ cấu trúc YAML
  - Python 2.4 trở lên

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
  - action_plugins:

- Các thiết lập khác thao khảo tại đây: http://docs.ansible.com/ansible/intro_configuration.html


##5. Module

##6. Playbooks.

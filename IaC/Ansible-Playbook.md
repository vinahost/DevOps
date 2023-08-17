# Ansible là gì?

![Ansible - Playbook](/Image/Ansible-Playbook.jpg)

**Ansible** là một công cụ tự động hóa cơ sở hạ tầng mã nguồn mở, giúp bạn quản lý và cấu hình các máy chủ từ xa. **Ansible** sử dụng một kiến trúc **agentless**, nghĩa là nó không yêu cầu cài đặt bất kỳ phần mềm bổ sung nào trên các máy chủ được quản lý. Thay vào đó, **Ansible** sử dụng giao thức **SSH** để giao tiếp với các máy chủ và thực hiện các tác vụ.

**Ansible** được sử dụng để quản lý một loạt các tác vụ, bao gồm:

- Cài đặt phần mềm
- Cấu hình hệ thống
- Khởi động và dừng dịch vụ
- Quản lý bảo mật
- Phân phối thay đổi

## Lợi ích của việc sử dụng Ansible

- Dễ sử dụng: Ansible được thiết kế để dễ sử dụng, ngay cả đối với những người không có kinh nghiệm về tự động hóa hệ thống.
- Mạnh mẽ: Ansible có thể được sử dụng để quản lý một loạt các tác vụ, từ cài đặt phần mềm đến cấu hình hệ thống.
- Linh hoạt: Ansible có thể được sử dụng để quản lý các máy chủ từ nhiều nền tảng khác nhau, bao gồm Linux, Windows và macOS.
- Mở rộng: Ansible có thể được mở rộng bằng cách sử dụng các mô-đun và công cụ của bên thứ ba.

## Hạn chế

- Nếu không phải triển khai 1 hệ thống đủ lớn, hoặc chỉ phải thực hiện trên 1, 2 server thì thực sự không cần thiết dùng đến
- Viết ra kịch bản có khi tiêu tốn nhiều thời gian hơn việc bạn thực hiện nó bằng lệnh.

## Thành phần trong ansible

1. **Play-book** - Là nơi bạn sẽ khai báo kịch bản chạy cho server
2. **Tasks** -  Là những công việc nhỏ trong cuốn sổ Playbooks trên
3. **Inventory** - Khai báo địa chỉ server cần được setup
4. **Modules** - Những chức năng hỗ trợ cho việc thực thi tasks dễ và đang dạng

## PLAYBOOKS

**Ansible Playbook** là gì ?

**Ansible Playbook** là một tệp văn bản được viết bằng định dạng **YAML** chứa các hướng dẫn về cách tự động hóa các tác vụ trên các máy chủ. Các **playbook Ansible** thường được sử dụng để quản lý cấu hình, triển khai phần mềm và quản lý mạng.

Một **playbook Ansible** có thể chứa nhiều tác vụ, mỗi tác vụ thực hiện một nhiệm vụ cụ thể. Các tác vụ có thể được sắp xếp thành các nhóm để tổ chức mã. Các **playbook Ansible** có thể được chạy theo cách thủ công hoặc được lên lịch để chạy tự động.

**Ansible Playbook** là một cách hiệu quả để tự động hóa các tác vụ trên các máy chủ. Chúng dễ viết và dễ đọc, và chúng có thể được sử dụng để quản lý các máy chủ trên nhiều nền tảng.

- Trong playbooks, chúng ta sẽ xác định những gì cần phải làm. Hay nói cách khác là nơi ta sẽ viết kịch bản cho các con server.
- Trong playbooks sẽ chứa một tập hợn các activities (hoạt động) hay các tasks (nhiệm vụ) sẽ được chạy trên một hay một nhóm servers.
- Trong đó task là một hành động duy nhất được thực hiện trên server, ví dụ như cài gói service nào đó, hay bật tắt service.

```
# Simple Ansible Playbook1.yml
-
  name: Play 1
  hosts: localhost
  tasks:
    - name: Execute command "date"
      command: date
    - name: Execute script on server
      script: test_script.sh
    - name: Install httpd service
      yum:
        name: httpd
        state: present
    - name: Start web server
      service:
        name: httpd
        state: started
```

Trên đây là một **playbook** đơn giản chứa một kịch bản có tên **Play 1** (**name: Play 1**).

Có tổng cộng 4 nhiệm vụ cần được chạy cho **server**. Nhiệm vụ lần lượt là:

1. chạy lệnh date
1. chạy file test_script.sh
1. cài đặt dịch vụ httpd
1. start dịch vụ httpd vừa cài trên

Bạn để ý các thuộc tính **command, script, yum, service** là những **code** có sẵn do **ansible** cung cấp. **Module** hỗ trợ bạn viết và thực thi các nhiệm vụ một cách đơn giản hơn.

Chạy file ansible play-book

```
ansible-palybook -i hosts my_playbook.yml
```

## MODULES

**Ansible** cung cấp rất nhiều module

1. **System**: Bao gồm các module như User, Group, Hostname, Systemd, Service
2. **Commands**: Thường có module con như Command, Expect, Raw, Script, Shell
3. **Files**: Các module làm việc với file như Copy, Find, Lineinfile, Replace, v.v...
4. **Database**: Ansbile cũng support mạnh mẽ những module làm việc với DB như Mongodb, Mssql, Mysql, Postgresql, Proxysql, v.v...
5. **Cloud**: Ansible cũng không quên kết hợp với các dịch vụ clound nổi tiếng như Amazon, Google, Docker, Linode, VMware, Digital Ocean, v.v...

Và còn hàng trăm module khác đã được **ansible** cung cấp sẵn.

## INVENTORY

Đây là nơi sẽ chứa tên các con **server** hay địa chỉ **ip** mà bạn muốn thực thi.

```
# Sample Inventory File
Server1.company.com
Server2.company.com 

[mail]
Server3.company.com 
Server4.company.com 

[db]
Server5.company.com 
Server6.company.com 

[web]
Server7.company.com
Server8.company.com 

[all_servers:children]
mail
db
web
```

Bạn để ý cách khai báo với **[mail], [db], [web].** Đây là cách khai báo 1 **group** các **server** với nhau. **[mail]** là tên **group**. Trong **playbook**, nếu bạn muốn **file playbook** đó sẽ thực thi **group** các **server** liên quan đến **web**, bạn chỉ cần khai báo

```
hosts: web
```

Còn **[all_servers:children]** là cách khai báo **group** các **group** với nhau.

**Ansible** còn cung cấp một số **params** phục vụ cho việc truy cập vào **server** mà bạn đã khai báo trong **inventory file** dễ dàng hơn.

```
# DB Servers
sql_db1 ansible_host=sql01.xyz.com ansible_connection=ssh ansible_user=root ansible_ssh_pass=Lin$Pass
sql_db2 ansible_host=sql02.xyz.com ansible_connection=ssh ansible_user=root ansible_ssh_pass=Lin$Pass
```

## VARIABLES

Biến được sử dụng để lưu trữ các giá trị và có thể thay đổi giá trị được.

```
-
  name: Print car's information
  hosts: localhost
  vars:
    car_model: "BMW M3"
    country_name: USA
    title: "Systems Engineer"
  tasks:
    - name: Print my car model
      command: echo "My car's model is {{ car_model }}"

    - name: Print my country
      command: echo "I live in the {‌{ country_name }}"
```

Để khai báo biến, chúng ta sẽ sử dụng thuộc tính **vars** mà **ansible** đã cung cấp.
**car_model** sẽ là key, **"BMW M3"** sẽ là **value**. Bên dưới để sử dụng biến **car_model** ta sử dụng cặp dấu ngoặc nhọn và tên biến **{{ car_model }}**

## CONDITIONS

**Ansible** cũng cho phép bạn điều hướng lệnh chạy hay giới hạn phạm vi để run câu lệnh nào đó.

```
# Simple playbook.yml
-
  name: Toi da tot nghiep chua
  hosts: localhost
  vars:
    age: 25
  tasks:
    - name: 'bfd'
      command: echo "Toi chua tot nghiep"
      when: age < 22          
    - name: 'abc                    
      command: echo "Toi da tot nghiep"                     
      when: age >= 22
```

## REGISTER

**Register** giúp nhận kết quả trả về từ một câu lệnh. Sau đó ta có thể dùng kết quá trả về đó cho những câu lệnh chạy sau đó.

Ví dụ ta có bài toán như sau: kiểm tra trạng thái của **service httpd**, nếu **start** thất bại thì gửi **mail** thông báo cho **admin**.

```
# Sample ansible playbook.yml
-
  name: Check status of service and email if its down
  hosts: localhost
  tasks:
    - command: service httpd status
      register: command_output

    - mail:
        to: Admins 
        subject: Service Alert
        body: "Service is down"
      when: command_output.stdout.find("down") != -1
```

Nhờ vào thuộc tính **register**, kết quả trả về sẽ được chứa vào biến **command_output**. Từ đó ta sử dụng tiếp các thuộc tính của biến **command_output** là **stdout.find** để tìm chữ "**down**" có xuất hiện trong nội dung trả về không. Nếu không tìm thấy thì kết quả sẽ là **-1**.

## LOOPS

Nhưng nếu **server** yêu cầu cài thêm nhiều gói **service** khác như **mysql**, **php** thì sao. Như bình thường chúng ta sẽ viết như sau:

```
# Simple Ansible Playbook1.yml
-
  name: Install packages
  hosts: localhost
  tasks:
    - name: Install httpd service
      yum:
        name: httpd
    - name: Install mysql service
      yum:
        name: mysql
    - name: Install php service
      yum:
        name: php
```

Ở đây mới ví dụ 3 **service** cần cài mà phải viết lập lại các thuộc tính **name**, **module yum** đến 3 lần. Nếu **server** cần cài lên đến **100** gói **service** thì việc ngồi **copy/paste** cũng trở nên vấn đề đấy. Thay vào đó, chúng ta sẽ sử dụng chức năng **loops** mà **ansible** đã cung cấp để để viết.

```
# Simple Ansible Playbook1.yml
-
  name: Install packages
  hosts: localhost
  tasks:
    - name: Install all service
      yum: name="{{ item }}" state=present
      with_items:
        - httpd
        - mysql
        - php
```
 
**with_items** là một lệnh lặp, thực thi cùng một tác vụ nhiều lần. Mỗi lần chạy, nó lưu giá trị của từng thành phần trong biến **item**.

## ROLES

Nếu bạn có nhiều **server** hay nhiều **group server** và mỗi **server** thực thiện những **tasks** riêng biệt. Và khi này nếu viết tất cả vào cùng một **file playbook** thì **code** khá là xấu và khó để quản lý. **Ansible** đã cung cấp sẵn chức năng **roles**, về đơn giản nó sẽ giúp bạn phân chia khu vực với nhiệm vụ riêng biệt.

```
# Simple Ansible setup_application.yml
-
  name: Set firewall configurations
  hosts: web
  vars:
    http_port: 8081
    snmp_port: 160-161
    inter_ip_range: 192.0.2.0
    
  tasks:
    - firewalld:
        service: https
        permanent: true
        state: enabled
    - firewalld:
        port: "{{ http_port }}"/tcp
        permanent: true
        state: disabled
    - firewalld:
        port: "{{ snmp_port }}"/udp
        permanent: true
        state: disabled
    - firewalld:
        source: "{{ inter_ip_range }}"/24
        zone: internal
        state: enabled
```

Mục tiêu của file **playbook** `setup_application.yml` này là cấu hình tường lửa cho **group server** về **web**. Bây giờ chúng ta sẽ cắt nhỏ **file playbook** này ra thành những **file** có chức năng riêng biệt như **file** chỉ chưa định nghĩa biến, hay **file** chứa định nghĩa **tasks**. Trước khi cắt **file playbook** nhỏ gọn lại, ta cần tạo cấu trúc thư mục như sau để **ansible** nhận biết được các thành phần ta đã khai báo.

![Ansible Playbook](/Image/Ansible-Playbook01.png)

## Modules
### [1. File](https://docs.ansible.com/ansible/latest/modules/file_module.html)

Create **Folder/File**

```
- name: create folder
  file:
  	path: /home/app/name-folder
  	state: dicrectory

- name: create file
  file: 
  	path: /home/app/file.rb
  	state: touch
```

Link **Folder/File** - **change ownership**

```
- name: Create a symbolic link
  file:
    src: /file/to/link/to
    dest: /path/to/symlink
    state: link
    
- name: Change permisstion ownership
  file:
  	path: /home/app/folder
  	owner: app
  	group: app
```

### [2. Copy](https://docs.ansible.com/ansible/latest/modules/copy_module.html)

```
- name: Copy file with owner and permissions
  copy:
    src: /home/app/foo.conf
    dest: /etc/foo.conf
    owner: app
    group: app
    mode: '0644'
```

### [3. Template](https://docs.ansible.com/ansible/latest/modules/template_module.html)

```
- name: Template a file to /etc/files.conf
  template:
    src: /mytemplates/foo.j2
    dest: /etc/nginx/conf.d/default.conf
    owner: app
    group: app
    mode: '0644'
```

### [4. APT - Manages apt-packages](https://docs.ansible.com/ansible/latest/modules/apt_module.html)

```
- name: Install nginx  (state=present is optional)
  apt:
    name: nginx
    state: present
    
- name: Uninstall nginx
  apt:
    name: nginx
    state: absent
```

### [5. yum – Manages packages with the yum package manager](https://docs.ansible.com/ansible/latest/modules/yum_module.html)

```
- name: install the latest version of Apache
  yum:
    name: httpd
    state: latest
  
- name: ensure a list of packages installed
  yum:
    name: "{{ packages }}"
  vars:
    packages:
    - httpd
    - httpd-tools

- name: remove the Apache package
  yum:
    name: httpd
    state: absent
  
- name: upgrade all packages
  yum:
    name: '*'
    state: latest
```

### [6. Git](https://docs.ansible.com/ansible/latest/modules/git_module.html)

```
- name: git clone https://abc.com
  git: repo=https://abc.com dest=/home/app/folder
```

### [7. Lineinfile](https://docs.ansible.com/ansible/latest/modules/lineinfile_module.html)

```
- name: Add a line to a file if the file does not exist, without passing regexp
  lineinfile:
    path: /home/app/etc/hosts
    line: 127.0.0.0 development.site
    create: yes
```

### [8. blockinfile – Insert/update/remove a text block surrounded by marker lines](https://docs.ansible.com/ansible/latest/modules/blockinfile_module.html)

```
- name: Insert/Update eth0 configuration stanza in /etc/network/interfaces
        (it might be better to copy files into /etc/network/interfaces.d/)
  blockinfile:
    path: /etc/network/interfaces
    block: |
      iface eth0 inet static
          address 192.0.2.23
          netmask 255.255.255.0
```

### [9. Shell](https://docs.ansible.com/ansible/latest/modules/shell_module.html)

```
- name: Run a command that uses non-posix shell-isms (in this example /bin/sh doesn't handle redirection and wildcards together but bash does)
  shell: cat < /tmp/*txt
  args:
    executable: /bin/bash
```

### [10. Command](https://docs.ansible.com/ansible/latest/modules/command_module.html)

```
- name: return motd to registered var
  command: cat /etc/motd
  register: mymotd
```

### [11. stat – Retrieve file or file system status](https://docs.ansible.com/ansible/latest/modules/stat_module.html)

```
- name: Ansible check versions {{ ruby_version }} exists.
  stat:
    path: "{{ rbenv_root }}/versions/{{ ruby_version }}"
  register: dir_version

- name: Install ruby {{ ruby_version }}
  command: sudo -iu {{ user }} rbenv install {{ ruby_version }}
  when: dir_version.stat.exists == False 
  
================================================================================ 
 
- stat:
    path: /path/to/something
  register: sym

- debug:
    msg: "islnk isn't defined (path doesn't exist)"
  when: sym.stat.islnk is not defined
```

### [12. How to set and use sudo password for Ansible Vault](https://www.cyberciti.biz/faq/how-to-set-and-use-sudo-password-for-ansible-vault/)

First update your inventory file as follows:

```
[cluster:vars]
k_ver="linux-image-4.13.0-26-generic"
ansible_user=vivek  # ssh login user
ansible_become=yes  # use sudo 
ansible_become_method=sudo 
ansible_become_pass='{{ my_cluser_sudo_pass }}'
 
[cluster]
www1
www2
www3
db1
db2
cache1
cache2

```

* Next create a new encrypted data file named password.yml, run the following command:

```
ansible-vault create passwd.yml
```

* Set the password for vault. After providing a password, the tool will start whatever editor you have defined with $EDITOR. Append the following

```
my_cluser_sudo_pass: your_sudo_password_for_remote_servers
```

* Save and close the file in vi/vim. Finally run playbook as follows:

```
ansible-playbook -i inventory --ask-vault-pass --extra-vars '@passwd.yml' my.yml
```

* How to edit my encrypted file again

```
ansible-vault edit passwd.yml
```

* How to change password for my encrypted file

```
ansible-vault rekey passwd.yml
```

#### Summary
In short use following options for the ansible-playbook command with vault or without vault file:

* `-i inventory` : Set path to your inventory file.
* `--ask-vault-pass` : Ask for vault password
* `--extra-vars '@passwd.yml'` – Set extra variable. In this case set path to vault file named passwd.yml.
* `--ask-become-pass` : Ask for sudo password


## Directiory Layout

[Best Practices](https://docs.ansible.com/ansible/latest/user_guide/playbooks_best_practices.html)

```
production                # inventory file for production servers
staging                   # inventory file for staging environment

group_vars/
	group1.yml             # here we assign variables to particular groups
	group2.yml
host_vars/
   hostname1.yml          # here we assign variables to particular systems
   hostname2.yml

library/                  # if any custom modules, put them here (optional)
module_utils/             # if any custom module_utils to support modules, put them here (optional)
filter_plugins/           # if any custom filter plugins, put them here (optional)

site.yml                  # master playbook
webservers.yml            # playbook for webserver tier
dbservers.yml             # playbook for dbserver tier

roles/
    common/          # this hierarchy represents a "role"
      tasks/         #
        main.yml     #  <-- tasks file can include smaller files if warranted
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
	library/          # roles can also include custom modules
	module_utils/     # roles can also include custom module_utils
	lookup_plugins/   # or other types of plugins, like lookup in this case

    webtier/              # same kind of structure as "common" was above, done for the webtier role
    monitoring/           # ""
    fooapp/               # ""
Alternative Directory Layout
```


## NOTE

### 4.1.1. Inventory Parameters
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

### 4.1.2 Iventory Dynamic

## 4.2 `ansible.cfg`

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

Các thiết lập khác tham khảo [tại đây](http://docs.ansible.com/ansible/intro_configuration.html)


## Playbook chuẩn
```
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

**Trong đó:**

- production: giống file /etc/ansible/hosts, liệt kê group, host
- group_vars/*: đặt các biến chung cho cùng 1 nhóm, ví dụ [webservers] có biến listen_port: 80
- host_vars/*: đặt các biến riêng cho từng host
- roles/*: đặt các role, ví dụ các host trong [webservers] gọi đến role webtier
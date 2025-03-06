# ansible常用模块

- ping: 主机存活率性探测

```
~]# ansible all -m ping
192.168.1.69 | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python"
    }, 
    "changed": false, 
    "ping": "pong"
}
192.168.1.68 | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python"
    }, 
    "changed": false, 
    "ping": "pong"
}
192.168.1.67 | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python"
    }, 
    "changed": false, 
    "ping": "pong"
}
```

- command：在目标上执行命令，不支持管道符

```
~]# ansible all -m command -a "free -h"
192.168.1.69 | CHANGED | rc=0 >>
              total        used        free      shared  buff/cache   available
Mem:           1.8G        135M        1.4G        9.6M        251M        1.5G
Swap:          2.0G          0B        2.0G
192.168.1.68 | CHANGED | rc=0 >>
              total        used        free      shared  buff/cache   available
Mem:           1.8G        332M        1.2G        9.6M        217M        1.3G
Swap:          2.0G          0B        2.0G
192.168.1.67 | CHANGED | rc=0 >>
              total        used        free      shared  buff/cache   available
Mem:           1.8G        328M        1.2G        9.7M        212M        1.3G
Swap:          2.0G          0B        2.0G
```

- raw: 原始命令执行

```
~]# ansible all -m raw -a 'yum install lrzsz -y'
```

- shell： 在目标上执行命令，支持管道

```
~]# ansible all -m shell -a 'echo "12345"|passwd root --stdin'
192.168.1.69 | CHANGED | rc=0 >>
更改用户 root 的密码 。
passwd：所有的身份验证令牌已经成功更新。
192.168.1.68 | CHANGED | rc=0 >>
更改用户 root 的密码 。
passwd：所有的身份验证令牌已经成功更新。
192.168.1.67 | CHANGED | rc=0 >>
更改用户 root 的密码 。
passwd：所有的身份验证令牌已经成功更新。
```

- script: 传输本地脚本后在远程节点上运行

```
~]# ansible all -m script -a './test.sh'
192.168.1.69 | CHANGED => {
    "changed": true, 
    "rc": 0, 
    "stderr": "Shared connection to 192.168.1.69 closed.\r\n", 
    "stderr_lines": [
        "Shared connection to 192.168.1.69 closed."
    ], 
    "stdout": "", 
    "stdout_lines": []
}
192.168.1.68 | CHANGED => {
    "changed": true, 
    "rc": 0, 
    "stderr": "Shared connection to 192.168.1.68 closed.\r\n", 
    "stderr_lines": [
        "Shared connection to 192.168.1.68 closed."
    ], 
    "stdout": "", 
    "stdout_lines": []
}
192.168.1.67 | CHANGED => {
    "changed": true, 
    "rc": 0, 
    "stderr": "Shared connection to 192.168.1.67 closed.\r\n", 
    "stderr_lines": [
        "Shared connection to 192.168.1.67 closed."
    ], 
    "stdout": "", 
    "stdout_lines": []
}
```

- archive: 对远程主机的文件进行归档

```
~]# ansible all -m archive -a "path=testdir dest=testdir.tar.gz"
192.168.1.69 | CHANGED => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python"
    }, 
    "archived": [
        "testdir/test.txt"
    ], 
    "arcroot": "/", 
    "changed": true, 
    "dest": "testdir.tar.gz", 
    "expanded_exclude_paths": [], 
    "expanded_paths": [
        "testdir"
    ], 
    "gid": 0, 
    "group": "root", 
    "missing": [], 
    "mode": "0644", 
    "owner": "root", 
    "size": 130, 
    "state": "file", 
    "uid": 0
}
192.168.1.68 | CHANGED => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python"
    }, 
    "archived": [
        "testdir/test.txt"
    ], 
    "arcroot": "/", 
    "changed": true, 
    "dest": "testdir.tar.gz", 
    "expanded_exclude_paths": [], 
    "expanded_paths": [
        "testdir"
    ], 
    "gid": 0, 
    "group": "root", 
    "missing": [], 
    "mode": "0644", 
    "owner": "root", 
    "size": 130, 
    "state": "file", 
    "uid": 0
}
192.168.1.67 | CHANGED => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python"
    }, 
    "archived": [
        "testdir/test.txt"
    ], 
    "arcroot": "/", 
    "changed": true, 
    "dest": "testdir.tar.gz", 
    "expanded_exclude_paths": [], 
    "expanded_paths": [
        "testdir"
    ], 
    "gid": 0, 
    "group": "root", 
    "missing": [], 
    "mode": "0644", 
    "owner": "root", 
    "size": 130, 
    "state": "file", 
    "uid": 0
}
```

- blockinfile – 插入/更新/删除由标记线包围的文本块
  
```
~]# ansible all -m blockinfile -a "path=/etc/issue block='centos7\nyes'"
192.168.1.69 | CHANGED => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python"
    }, 
    "changed": true, 
    "msg": "Block inserted"
}
192.168.1.68 | CHANGED => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python"
    }, 
    "changed": true, 
    "msg": "Block inserted"
}
192.168.1.67 | CHANGED => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python"
    }, 
    "changed": true, 
    "msg": "Block inserted"
}
[root@c7-node01 ~]# cat /etc/issue
\S
Kernel \r on an \m

# BEGIN ANSIBLE MANAGED BLOCK
centos7
yes
# END ANSIBLE MANAGED BLOCK
```

- copy: 复制本地文件到远程

```
 ~]# ansible all -m copy -a "src=OpenJDK17U-jdk_x64_linux_hotspot_17.0.14_7.tar.gz dest=/tmp owner=root owner=root mode=0644"
192.168.1.69 | CHANGED => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python"
    }, 
    "changed": true, 
    "checksum": "f6958e5f961401074e2008b5b40d2b59d0bff08a", 
    "dest": "/tmp/OpenJDK17U-jdk_x64_linux_hotspot_17.0.14_7.tar.gz", 
    "gid": 0, 
    "group": "root", 
    "md5sum": "5c62406287656f8cc9d3488cdcd3e6bd", 
    "mode": "0644", 
    "owner": "root", 
    "size": 191943794, 
    "src": "/root/.ansible/tmp/ansible-tmp-1741229647.02-3034-200058529801125/source", 
    "state": "file", 
    "uid": 0
}
192.168.1.68 | CHANGED => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python"
    }, 
    "changed": true, 
    "checksum": "f6958e5f961401074e2008b5b40d2b59d0bff08a", 
    "dest": "/tmp/OpenJDK17U-jdk_x64_linux_hotspot_17.0.14_7.tar.gz", 
    "gid": 0, 
    "group": "root", 
    "md5sum": "5c62406287656f8cc9d3488cdcd3e6bd", 
    "mode": "0644", 
    "owner": "root", 
    "size": 191943794, 
    "src": "/root/.ansible/tmp/ansible-tmp-1741229647.02-3033-96414164659498/source", 
    "state": "file", 
    "uid": 0
}
192.168.1.67 | CHANGED => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python"
    }, 
    "changed": true, 
    "checksum": "f6958e5f961401074e2008b5b40d2b59d0bff08a", 
    "dest": "/tmp/OpenJDK17U-jdk_x64_linux_hotspot_17.0.14_7.tar.gz", 
    "gid": 0, 
    "group": "root", 
    "md5sum": "5c62406287656f8cc9d3488cdcd3e6bd", 
    "mode": "0644", 
    "owner": "root", 
    "size": 191943794, 
    "src": "/root/.ansible/tmp/ansible-tmp-1741229647.34-3031-56264868402450/source", 
    "state": "file", 
    "uid": 0
}
```

- fetch: 抓去远程文件到本地

```
~]# ansible all -m fetch -a "src=/tmp/OpenJDK17U-jdk_x64_linux_hotspot_17.0.14_7.tar.gz dest=/data/bak"
192.168.1.69 | CHANGED => {
    "changed": true, 
    "checksum": "f6958e5f961401074e2008b5b40d2b59d0bff08a", 
    "dest": "/data/bak/192.168.1.69/tmp/OpenJDK17U-jdk_x64_linux_hotspot_17.0.14_7.tar.gz", 
    "md5sum": "5c62406287656f8cc9d3488cdcd3e6bd", 
    "remote_checksum": "f6958e5f961401074e2008b5b40d2b59d0bff08a", 
    "remote_md5sum": null
}
192.168.1.68 | CHANGED => {
    "changed": true, 
    "checksum": "f6958e5f961401074e2008b5b40d2b59d0bff08a", 
    "dest": "/data/bak/192.168.1.68/tmp/OpenJDK17U-jdk_x64_linux_hotspot_17.0.14_7.tar.gz", 
    "md5sum": "5c62406287656f8cc9d3488cdcd3e6bd", 
    "remote_checksum": "f6958e5f961401074e2008b5b40d2b59d0bff08a", 
    "remote_md5sum": null
}
192.168.1.67 | CHANGED => {
    "changed": true, 
    "checksum": "f6958e5f961401074e2008b5b40d2b59d0bff08a", 
    "dest": "/data/bak/192.168.1.67/tmp/OpenJDK17U-jdk_x64_linux_hotspot_17.0.14_7.tar.gz", 
    "md5sum": "5c62406287656f8cc9d3488cdcd3e6bd", 
    "remote_checksum": "f6958e5f961401074e2008b5b40d2b59d0bff08a", 
    "remote_md5sum": null
}
~]# tree /data/bak/
/data/bak/
├── 192.168.1.67
│   └── tmp
│       └── OpenJDK17U-jdk_x64_linux_hotspot_17.0.14_7.tar.gz
├── 192.168.1.68
│   └── tmp
│       └── OpenJDK17U-jdk_x64_linux_hotspot_17.0.14_7.tar.gz
└── 192.168.1.69
    └── tmp
        └── OpenJDK17U-jdk_x64_linux_hotspot_17.0.14_7.tar.gz
```
- file: 管理文件和文件属性

```
~]# ansible all -m file -a 'path=/data/apps/1.txt mode=u+rw,g-rw,o-rw state=touch'
~]# ansible all -m file -a 'path=/data/apps mode=u+rw,g+rw,o-rw state=directory'
192.168.1.69 | CHANGED => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python"
    }, 
    "changed": true, 
    "gid": 0, 
    "group": "root", 
    "mode": "0771", 
    "owner": "root", 
    "path": "/data/apps", 
    "size": 6, 
    "state": "directory", 
    "uid": 0
}
192.168.1.68 | CHANGED => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python"
    }, 
    "changed": true, 
    "gid": 1001, 
    "group": "app", 
    "mode": "0771", 
    "owner": "app", 
    "path": "/data/apps", 
    "size": 4096, 
    "state": "directory", 
    "uid": 1001
}
192.168.1.67 | CHANGED => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python"
    }, 
    "changed": true, 
    "gid": 1002, 
    "group": "app", 
    "mode": "0771", 
    "owner": "app", 
    "path": "/data/apps", 
    "size": 4096, 
    "state": "directory", 
    "uid": 1002
}
```

- lineinfile – 管理文本文件中的行

```
~]# ansible all -m lineinfile -a "path=/etc/issue regex='centos*' state=absent"
~]# ansible all -m lineinfile -a "path=/etc/selinux/config regex='^SELINUX=' line=SELINUX=disabled"
192.168.1.68 | CHANGED => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python"
    }, 
    "backup": "", 
    "changed": true, 
    "msg": "line replaced"
}
192.168.1.69 | CHANGED => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python"
    }, 
    "backup": "", 
    "changed": true, 
    "msg": "line replaced"
}
192.168.1.67 | CHANGED => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python"
    }, 
    "backup": "", 
    "changed": true, 
    "msg": "line replaced"
}
```

- synchronize: 同步文件到远程主机

```
~]# ansible all -m synchronize -a "src=/data/apps/springboot-app-0.0.1-SNAPSHOT.jar dest=/data/apps/springboot-app-0.0.1-SNAPSHOT.jar"
192.168.1.67 | SUCCESS => {
    "changed": false, 
    "cmd": "/usr/bin/rsync --delay-updates -F --compress --archive --rsh=/usr/bin/ssh -S none -o StrictHostKeyChecking=no -o UserKnownHostsFile=/dev/null --out-format=<<CHANGED>>%i %n%L /data/apps/springboot-app-0.0.1-SNAPSHOT.jar 192.168.1.67:/data/apps/springboot-app-0.0.1-SNAPSHOT.jar", 
    "msg": "", 
    "rc": 0, 
    "stdout_lines": []
}
192.168.1.68 | CHANGED => {
    "changed": true, 
    "cmd": "/usr/bin/rsync --delay-updates -F --compress --archive --rsh=/usr/bin/ssh -S none -o StrictHostKeyChecking=no -o UserKnownHostsFile=/dev/null --out-format=<<CHANGED>>%i %n%L /data/apps/springboot-app-0.0.1-SNAPSHOT.jar 192.168.1.68:/data/apps/springboot-app-0.0.1-SNAPSHOT.jar", 
    "msg": "<f..t...... springboot-app-0.0.1-SNAPSHOT.jar\n", 
    "rc": 0, 
    "stdout_lines": [
        "<f..t...... springboot-app-0.0.1-SNAPSHOT.jar"
    ]
}
192.168.1.69 | CHANGED => {
    "changed": true, 
    "cmd": "/usr/bin/rsync --delay-updates -F --compress --archive --rsh=/usr/bin/ssh -S none -o StrictHostKeyChecking=no -o UserKnownHostsFile=/dev/null --out-format=<<CHANGED>>%i %n%L /data/apps/springboot-app-0.0.1-SNAPSHOT.jar 192.168.1.69:/data/apps/springboot-app-0.0.1-SNAPSHOT.jar", 
    "msg": "<f+++++++++ springboot-app-0.0.1-SNAPSHOT.jar\n", 
    "rc": 0, 
    "stdout_lines": [
        "<f+++++++++ springboot-app-0.0.1-SNAPSHOT.jar"
    ]
}
```

- unarchive: 解压本地文件到远程主机

```
 ~]# ansible all -m unarchive -a "src=OpenJDK17U-jdk_x64_linux_hotspot_17.0.14_7.tar.gz dest=/usr/local" # src=https://example.com/example.zip


192.168.1.69 | CHANGED => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python"
    }, 
    "changed": true, 
    "dest": "/usr/local", 
    "extract_results": {
        "cmd": [
            "/usr/bin/gtar", 
            "--extract", 
            "-C", 
            "/usr/local", 
            "-z", 
            "-f", 
            "/root/.ansible/tmp/ansible-tmp-1741231264.91-4314-266470145558557/source"
        ], 
        "err": "", 
        "out": "", 
        "rc": 0
    }, 
    "gid": 0, 
    "group": "root", 
    "handler": "TgzArchive", 
    "mode": "0755", 
    "owner": "root", 
    "size": 152, 
    "src": "/root/.ansible/tmp/ansible-tmp-1741231264.91-4314-266470145558557/source", 
    "state": "directory", 
    "uid": 0
}
192.168.1.68 | CHANGED => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python"
    }, 
    "changed": true, 
    "dest": "/usr/local", 
    "extract_results": {
        "cmd": [
            "/usr/bin/gtar", 
            "--extract", 
            "-C", 
            "/usr/local", 
            "-z", 
            "-f", 
            "/root/.ansible/tmp/ansible-tmp-1741231264.27-4313-175702533487511/source"
        ], 
        "err": "", 
        "out": "", 
        "rc": 0
    }, 
    "gid": 0, 
    "group": "root", 
    "handler": "TgzArchive", 
    "mode": "0755", 
    "owner": "root", 
    "size": 152, 
    "src": "/root/.ansible/tmp/ansible-tmp-1741231264.27-4313-175702533487511/source", 
    "state": "directory", 
    "uid": 0
}
192.168.1.67 | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python"
    }, 
    "changed": false, 
    "dest": "/usr/local", 
    "gid": 0, 
    "group": "root", 
    "handler": "TgzArchive", 
    "mode": "0755", 
    "owner": "root", 
    "size": 178, 
    "src": "/root/.ansible/tmp/ansible-tmp-1741231269.52-4311-83132205608791/source", 
    "state": "directory", 
    "uid": 0
}
```

- get_url: 从 HTTP、HTTPS 或 FTP 下载文件到节点

```
 ~]# ansible all -m get_url -a 'url=https://dlcdn.apache.org/maven/maven-3/3.8.8/binaries/apache-maven-3.8.8-bin.tar.gz dest=/tmp'
```

- yum: yum 软件包管理

```
~]# ansible all -m yum -a 'name=wget state=present'
192.168.1.68 | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python"
    }, 
    "changed": false, 
    "msg": "", 
    "rc": 0, 
    "results": [
        "wget-1.14-18.el7_6.1.x86_64 providing wget is already installed"
    ]
}
192.168.1.69 | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python"
    }, 
    "changed": false, 
    "msg": "", 
    "rc": 0, 
    "results": [
        "wget-1.14-18.el7_6.1.x86_64 providing wget is already installed"
    ]
}
192.168.1.67 | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python"
    }, 
    "changed": false, 
    "msg": "", 
    "rc": 0, 
    "results": [
        "wget-1.14-18.el7_6.1.x86_64 providing wget is already installed"
    ]
}
```

- cron: 定时计划任务

```
~]# ansible all -m cron -a 'name="sync ntp time" minute=*/3 job="ntpdate ntp.aliyun.com > /dev/null"'
```
- timezone: 时区设置

```
 ~]# ansible all -m timezone -a 'name=Asia/Shanghai'
```

- user： 用户管理

```
 ~]# ansible all -m user -a 'name="dengyouf" shell="/bin/bash" uid=1000 state=present'
```

# linux\_sec\_check\_ai\_helper
Linux Server Emergency Security Check Script. 

# 参考
此工具的编写主要参考了以下两款工具及个人工作经验总结而成，特别是 `al0ne/LinuxCheck`.

```
* https://github.com/al0ne/LinuxCheck 
* https://github.com/grayddq/GScan
* https://github.com/CISOfy/lynis
* https://www.chkrootkit.org/
```

Tips. `lynis` & `chkrootkit` 作为独立工具，可单独使用。

# 脚本说明
* 采用系统原生的命令，减少外部依赖；
* 输出上做了很多的简化，展现形式个人认为也有很大的提升；`ai helps`
* 以往脚本有较多的`信息采集`，涉及大量的文件查找，磁盘 I/O 比较慢，对于安全应急并不适合；因此，做了简化 & 拆分 auto/depth；

Tips：脚本支持 Centos、Debian，主要是在 Ubuntu 上进行测试。

# Helper
```
# bash linux_sec_check.sh
[*] =======================================================================================  
[*] \ Linux Emergency Response/Information Gathering/Vulnerability Detection Script V0.1 /   
[*] =======================================================================================  
[*] # System Support: Centos、Debian                                                      #  
[*] # Author: https://github.com/al0ne                                                    #  
[*] # Modify by: https://github.com/starnightcyber/linux_sec_check                        #  
[*] # Update: 2024-04 ~ 2024-06                                                           #  
[*] # Refer:                                                                              #  
[*] #   0.LinuxCheck https://github.com/al0ne/LinuxCheck                                  #  
[*] #   1.Gscan https://github.com/grayddq/GScan                                          #  
[*] #   2.Lynis https://github.com/CISOfy/lynis                                           #  
[*] =======================================================================================  

[*]The running script is linux_sec_check.sh 
[*]############ 0.校验系统 & 权限（仅支持 Debian/Ubuntu，且需 root 权限运行） ############ 
[+]当前为root权限 
[+]OS:Debian 
[+]运行环境检测通过 

usage:  
	 -v --version	 	 show script version.
 	 -a --auto		 run linux_sec_check.
	 -d --depth		 run linux_sec_check in deep mode.
	 -h --help		 print helper.
```

# Sample
```
[*]『系统常见软件安装』 
--------------------------------------------------------------------------------------------
| Software Name   | Path                                | MD5                              |
--------------------------------------------------------------------------------------------
| curl            | /usr/bin/curl                       | b194675c8ea858f2ed21214e9bbfc16b |
| wget            | /usr/bin/wget                       | c3d53e47e50f2f61016331da435b3764 |
| mysql           | /usr/bin/mysql                      | e954cca644ac8604c72cfc2713c6c7ca |
| nginx           | /usr/sbin/nginx                     | 2b275dfbc111c2126facc12391503bdc |
| docker          | /usr/bin/docker                     | ab9d7fd6e00364e85f992e35e50269ce |
| psql            | /usr/bin/psql                       | 5f99bff2c1cc3f0b79cb237f6d0e90f8 |
--------------------------------------------------------------------------------------------
```
```
[*]『SSH爆破 IPs Top10』 
IP Address           Attempts
-------------------  --------
196.245.250.10       8476    
193.32.162.4         2767    
103.78.12.10         1527    
45.183.247.34        1296    
137.184.18.250       1251    
124.152.99.57        995     
159.65.136.167       908     
218.92.41.172        824     
168.76.123.59        817     
2.57.122.87          766     
```

# 功能

| Category             | Item                                     |
|--------------------|---------------------------------------------|
| 基础信息采集       | 『系统信息』                                |
|                    | 『CPU使用率』                               |
|                    | 『内存使用』                                |
|                    | 『磁盘空间使用情况』                        |
|                    | 『硬盘挂载』                                |
|                    | 『系统常见软件安装』                        |
| 进程/软件检查      | 『CPU占用 Top10』                           |
|                    | 『内存占用 Top10』                          |
|                    | 『木马/挖矿检查』                           |
| 网络/流量检查      | 『网卡信息』                                |
|                    | 『网卡混杂模式』                            |
|                    | 『路由转发』                                |
|                    | 『对外开放高危通用服务』                    |
|                    | 『DNS 配置』                                |
|                    | 『路由表』                                  |
|                    | 『TCP连接状态统计计数』                     |
| 任务计划检查       | 『user crontasks』                          |
|                    | 『Crontab Backdoor』                        |
|                    | 『/etc/cron.*』                             |
| 环境变量检查       | 『env』                                     |
|                    | 『PATH』                                    |
| 用户信息检查       | 『可登陆用户』                              |
|                    | 『/etc/passwd 文件修改日期』                |
|                    | 『Root权限（非root）账号』                  |
|                    | 『sudoers(请注意NOPASSWD)』                 |
|                    | 『登录信息 - w』                            |
|                    | 『登录信息 - lastlog/filter never logged in』|
|                    | 『登录记录 - 计数』                         |
|                    | 『登录记录 - last』                         |
| 服务状态检查       | 『正在运行的Service』                       |
|                    | 『multi-user.target 运行级别服务』          |
|                    | 『用户自定义 Systemd 服务单元』            |
| Bash 配置检查      | 『History 文件』                            |
|                    | 『History 危险命令』                        |
|                    | 『bash反弹shell』                           |
| 文件检查           | 『系统文件修改时间』                        |
|                    | 『alias 检查』                              |
|                    | 『SUID』                                    |
|                    | 『lsof -L1/进程存在但文件已经没有了』       |
|                    | 『近七天文件改动 mtime』                    |
|                    | 『近七天文件改动 ctime』                    |
|                    | 『大文件>200MB』                            |
|                    | 『敏感文件』                                |
|                    | 『可疑黑客文件』                            |
|                    | 『...隐藏文件』                             |
| Rootkit检查        | 『lsmod 可疑模块』                          |
|                    | 『Rootkit 内核模块』                        |
|                    | 『可疑的.ko模块』                           |
| SSH检查            | 『SSH 监听 & 链接』                         |
|                    | 『SSH爆破 IPs Top10』                       |
|                    | 『SSHD:/usr/sbin/sshd 配置文件权限 A/M/C Time』|
|                    | 『SSH 后门配置』                            |
|                    | 『SSH 软连接后门』                          |
|                    | 『SSH inetd后门检查』                       |
|                    | 『SSH Key』                                 |
| Webshell检查       | 『PHP webshell查杀』                        |
|                    | 『JSP webshell查杀』                        |
| 供应链投毒检测     | 『Python2 pip 检测』                        |
|                    | 『Python3 pip 检测』                        |
| 挖矿检测           | 『常规挖矿进程检测』                        |
|                    | 『Ntpclient 挖矿木马检测』                  |
|                    | 『WorkMiner 挖矿木马检测』                  |

Note.表格由 GPT 生成。

# MIT LICENSE
Follow `al0ne/LinuxCheck`.

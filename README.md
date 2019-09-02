# EWP_OMS
>自动化运维系统（saltstack+django+bootstrap），QQ群342844540，博客http://ywzhou.blog.51cto.com/2785388/d-9

## 数据库:

>进入MySQL Command Line Client

>show databases;

>create database ewp_oms character set utf8;

>mysql> grant all on ewp_oms.* to 'admin'@'localhost' identified by "abc@123";

>mysql> flush privileges;

>use ewp_oms;

>进入项目路径执行数据库同步

>python manage.py migrate

>创建管理员账号(登陆页面及后台管理)

>python manage.py createsuperuser


##  目前已实现功能：

### CMDB资产管理：

>－机房：设备统计

![](https://raw.githubusercontent.com/ywzhou123/EWP_OMS/master/static/screen/idc.png)

>－硬件服务器：详细信息、主机统计、过滤、数据采集（grains)

![](https://raw.githubusercontent.com/ywzhou123/EWP_OMS/master/static/screen/server.png)

>－操作主机：详细信息、过滤、搜索、初始化安装(salt-ssh minion模块）、数据采集(grains)

![](https://raw.githubusercontent.com/ywzhou123/EWP_OMS/master/static/screen/host.png)

>－网络设备：WEB链接、过滤

![](https://raw.githubusercontent.com/ywzhou123/EWP_OMS/master/static/screen/net.png)

>－操作系统：主机统计

![](https://raw.githubusercontent.com/ywzhou123/EWP_OMS/master/static/screen/system.png)

### SALT配置管理：

>－命令管理：管理salt module 和 命令，通过'doc.runner','doc.wheel','doc.execution'命令自动采集模块、命令、及帮助信息；
非内置模块需要手动添加，如svn

![](https://raw.githubusercontent.com/ywzhou123/EWP_OMS/master/static/screen/command.png)

![](https://raw.githubusercontent.com/ywzhou123/EWP_OMS/master/static/screen/help.png)

>－Minion管理：管理key的接受和删除，新增Minions表，用于存储minion(key)、状态、grains、pillar等信息，CMDB中的数据可以根据IP来调取minion对象；
还可以操作自定义grain、pillar等数据。

![](https://raw.githubusercontent.com/ywzhou123/EWP_OMS/master/static/screen/minions.png)

![](https://raw.githubusercontent.com/ywzhou123/EWP_OMS/master/static/screen/grains.png)

>－接口配置：SALT MASTER端RSET API接口信息，关联机房；新增配置管理，实时获取环境、配置等信息。

![](https://raw.githubusercontent.com/ywzhou123/EWP_OMS/master/static/screen/saltserver.png)
        
![](https://raw.githubusercontent.com/ywzhou123/EWP_OMS/master/static/screen/config.png)

>－命令执行：

－目标选择：client、target类型均可选，同步异步命令均可执行，目标主机通过manage.status命令获取在线minions；

－命令选择：模块+命令选择框级联；

－结果展示：命令通过异步执行时，先展示JID，再向后台请求JID详细结果并使用jsonformat格式化展示；结果保存在mysql中；

－命令结果：读取mysql中的执行命令历史记录并展示；结果为空时条目背景为红色否则绿色。

![](https://raw.githubusercontent.com/ywzhou123/EWP_OMS/master/static/screen/execute.png)

>－本地文件：WEB端的本地文件管理（media目录），实现返回上层目录、创建、删除、改名、上传、下载（有些文件要右键另存不知为啥）、保存功能（移动不太好加，涉及SVN问题），
                   以及结合SVN，实现版本信息显示、提交（增删改）、更新、还原、签出功能；对文件读取做了后缀格式限制和文件大小限制；
                   还可以增加推送功能（cp.get_url）；需要安装SVN模块：pip install svn
                   
![](https://raw.githubusercontent.com/ywzhou123/EWP_OMS/master/static/screen/localfile.png)

>－远程文件：通过salt实现对客户端主机的文件系统管理，可以创建、删除、重命名文件或目录，可以修改文件内容，对文件的访问做了大小和格式的限制；
              如果是SVN副本还会显示版本信息；调用了代码发布功能，实现SVN副本的提交和更新。
              
－目标选择：根据条件过滤或搜索目标主机；

－文件查看：搜索路径搜索目标主机文件，实现返回上层（..)、判断是目录还是文件、文件内容展示，用的是实时返回，需要对文件格式、大小做限制；
                    计划增加字符替换功能或保存功能（对网络和后端稍有压力）
                    
![](https://raw.githubusercontent.com/ywzhou123/EWP_OMS/master/static/screen/remotefile.png)

>－代码发布：项目开发一般使用svn或git，这里以SVN项目发布为例，新建项目（项目名称、项目主机、项目路径、SVN地址、SVN账号、SVN密码、状态、信息）；
              刷新页面会处理每个项目，获取本地副本信息、如未发布则checkout、如没未安装SVN则pkg.install；可对单个项目进行提交和更新；
              SVN模块命令需要手动添加：https://docs.saltstack.com/en/latest/ref/modules/all/salt.modules.svn.html
              
![](https://raw.githubusercontent.com/ywzhou123/EWP_OMS/master/static/screen/deploy.png)

>－应用部署：本地文件管理页面进行sls模块编写、文件上传、提交，代码发布页面将SVN发布到master指定路径（/srv/salt），应用部署页面
动态获取SaltMaster的环境及其sls模块，批量选择主机通过state.sls进行安装并显示、记录结果；需要进一步细化，如增加test=true功能，增加pillar值输入，改为异步执行等。

![](https://raw.githubusercontent.com/ywzhou123/EWP_OMS/master/static/screen/state.png)

### 操作记录：

>－执行记录：保存salt命令执行记录。

![](https://raw.githubusercontent.com/ywzhou123/EWP_OMS/master/static/screen/result.png)

![](https://raw.githubusercontent.com/ywzhou123/EWP_OMS/master/static/screen/resultinfo.png)

### Zabbix监控
>Zabbix3.0下载地址：http://www.zabbix.com/download.php
>API接口文档地址：https://www.zabbix.com/documentation/3.0/manual/api
>下载Zabbix Appliance Zabbix 3.0 LTS Installation CD/DVD (.iso)进行快速安装
>在EWP_OMS\config.ini中定义相关连接参数

## Host列表：
>目前实现了简单的添加监控主机功能（后续会和cmdb、salt整合），及显示监控主机信息，点击相关对象链接跳转到对象详细页面
![](https://raw.githubusercontent.com/ywzhou123/EWP_OMS/master/static/screen/zbx-host.png)

## Item列表：
>显示监控项信息，点击名称显示其监控图，方法是直接调取zabbix服务端img图片，默认1小时，可选择时间范围及手动刷新
![](https://raw.githubusercontent.com/ywzhou123/EWP_OMS/master/static/screen/zbx-item.png)

>点击最近数值LASTVALUE可以查看HighChart画的图，数据是通过ZBX API的history.get命令获取。
![](https://raw.githubusercontent.com/ywzhou123/EWP_OMS/master/static/screen/zbx-history.png)

## Graph列表：
>同样是直接调取zabbix服务端graph的img图片进行显示，可选择时间范围及手动刷新
![](https://raw.githubusercontent.com/ywzhou123/EWP_OMS/master/static/screen/zbx-graph.png)

## Template列表：
>等待中。。。

## Group列表
>等待中。。。

## 近期规划：

>虚拟化平台管理：Esxi XenServer KVM （hyper-V就算了，能不用win的地方尽量不用），实现模板管理、虚拟机管理、迁移复制管理、自动扩容管理等等

>Cobber初装(实测不太好用，对windows、esxi、xen、ubuntu等支持不太好，放弃)



## 长远规划：

>Docker容器

>OpenStack云

>ELK日志

##新增实时告警

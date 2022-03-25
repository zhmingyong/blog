# harbor 2.4.2离线安装

> 之前安装过harbor 1.12，所以，安装2.1.0，也就轻车熟路了。前提当然是先安装好docker及docker-compose哟~

# 一，下载

[https://github.com/goharbor/harbor/releases/download/v2.4.2/harbor-online-installer-v2.4.2.tgz](https://github.com/goharbor/harbor/releases/download/v2.4.2/harbor-online-installer-v2.4.2.tgz)

# 二，解压

下载完成，上传服务器，解压。

```shell
[root@gitlab01 harbor]# ll
-rw-r--r--. 1 root root      3361 3月  15 11:48 common.sh
-rw-r--r--. 1 root root 630108360 3月  15 11:49 harbor.v2.4.2.tar.gz
-rw-r--r--. 1 root root      9668 3月  15 11:48 harbor.yml.tmpl
-rwxr-xr-x. 1 root root      2500 3月  15 11:48 install.sh
-rw-r--r--. 1 root root     11347 3月  15 11:48 LICENSE
-rwxr-xr-x. 1 root root      1881 3月  15 11:48 prepare
```

# 三，调整配置

从harbor.yml.tmpl复制一个harbor.yml，然后修改前面几行，自定义Hostname,port,禁用https，设置管理员密码。

```shell
# Configuration file of Harbor

# The IP address or hostname to access admin UI and registry service.
# DO NOT use localhost or 127.0.0.1, because Harbor needs to be accessed by external clients.
hostname: 192.168.11.101

# http related config
http:
  # port for http, default is 80. If https enabled, this port will redirect to https port
  port: 9999

# https related config
#https:
  # https port for harbor, default is 443
#  port: 443
  # The path of cert and key files for nginx
#  certificate: /your/certificate/path
#  private_key: /your/private/key/path

# # Uncomment following will enable tls communication between all harbor components
# internal_tls:
#   # set enabled to true means internal tls is enabled
#   enabled: true
#   # put your cert and key files on dir
#   dir: /etc/harbor/tls/internal

# Uncomment external_url if you want to enable external proxy
# And when it enabled the hostname will no longer used
# external_url: https://reg.mydomain.com:8433

# The initial password of Harbor admin
# It only works in first time to install harbor
# Remember Change the admin password from UI after launching Harbor.
harbor_admin_password: Harbor12345
```

# 四，安装并启动

```shell
[root@gitlab01 harbor]# ./install.sh 

[Step 0]: checking if docker is installed ...

Note: docker version: 20.10.12

[Step 1]: checking docker-compose is installed ...

Note: docker-compose version: 1.29.2

[Step 2]: loading Harbor images ...
0e1c91c1a3dc: Loading layer [==================================================>]  41.07MB/41.07MB
7abced50bfd2: Loading layer [==================================================>]  5.296MB/5.296MB
43ad630c07b8: Loading layer [==================================================>]  5.928MB/5.928MB
3457e72d7725: Loading layer [==================================================>]  14.47MB/14.47MB
4a079b4b377a: Loading layer [==================================================>]  29.29MB/29.29MB
7b180b7bf3b4: Loading layer [==================================================>]  22.02kB/22.02kB
7b678649a1be: Loading layer [==================================================>]  14.47MB/14.47MB
Loaded image: goharbor/notary-signer-photon:v2.4.2
4f650960b0dc: Loading layer [==================================================>]  5.832MB/5.832MB
967788659fd5: Loading layer [==================================================>]  4.096kB/4.096kB
30cc19bdf2d6: Loading layer [==================================================>]  3.072kB/3.072kB
9ccede1a4324: Loading layer [==================================================>]  47.85MB/47.85MB
86f4685faff0: Loading layer [==================================================>]  12.38MB/12.38MB
25af1cf9c44e: Loading layer [==================================================>]  61.02MB/61.02MB
Loaded image: goharbor/trivy-adapter-photon:v2.4.2
0c68eb0d6bac: Loading layer [==================================================>]    166MB/166MB
9df9445e80b3: Loading layer [==================================================>]  67.71MB/67.71MB
3017f66ba184: Loading layer [==================================================>]   2.56kB/2.56kB
9dc332519067: Loading layer [==================================================>]  1.536kB/1.536kB
d1baaa0c2bb4: Loading layer [==================================================>]  12.29kB/12.29kB
e911a97738e0: Loading layer [==================================================>]   2.62MB/2.62MB
6fbfaf19de14: Loading layer [==================================================>]  326.1kB/326.1kB
Loaded image: goharbor/prepare:v2.4.2
323ed00cae7e: Loading layer [==================================================>]  8.447MB/8.447MB
c3f8c4668352: Loading layer [==================================================>]  3.584kB/3.584kB
94b71cd07106: Loading layer [==================================================>]   2.56kB/2.56kB
5d5638decfe4: Loading layer [==================================================>]   75.6MB/75.6MB
48426521f41c: Loading layer [==================================================>]  5.632kB/5.632kB
bb977315930e: Loading layer [==================================================>]  97.28kB/97.28kB
2a80a5f64ee7: Loading layer [==================================================>]  11.78kB/11.78kB
0a5146355ed0: Loading layer [==================================================>]   76.5MB/76.5MB
7d5eac57d5fa: Loading layer [==================================================>]   2.56kB/2.56kB
Loaded image: goharbor/harbor-core:v2.4.2
f7cedf07c50c: Loading layer [==================================================>]  8.447MB/8.447MB
b2a916599ac7: Loading layer [==================================================>]  3.584kB/3.584kB
3bc2556f46f8: Loading layer [==================================================>]   2.56kB/2.56kB
a9cc192089ec: Loading layer [==================================================>]  86.96MB/86.96MB
9e9c53c1bd85: Loading layer [==================================================>]  87.75MB/87.75MB
Loaded image: goharbor/harbor-jobservice:v2.4.2
6111e09009c2: Loading layer [==================================================>]  7.222MB/7.222MB
Loaded image: goharbor/nginx-photon:v2.4.2
f4658d2c3aa3: Loading layer [==================================================>]  8.447MB/8.447MB
4b7bdcad271f: Loading layer [==================================================>]  18.13MB/18.13MB
cbb8c130a490: Loading layer [==================================================>]  4.608kB/4.608kB
f1367013643c: Loading layer [==================================================>]  18.93MB/18.93MB
Loaded image: goharbor/harbor-exporter:v2.4.2
a1b12e0ab8ea: Loading layer [==================================================>]  5.296MB/5.296MB
df6629a5ee28: Loading layer [==================================================>]  5.928MB/5.928MB
944fcde3a84b: Loading layer [==================================================>]  15.88MB/15.88MB
300e181c27cf: Loading layer [==================================================>]  29.29MB/29.29MB
87f01e78dde1: Loading layer [==================================================>]  22.02kB/22.02kB
8306283aa89d: Loading layer [==================================================>]  15.88MB/15.88MB
Loaded image: goharbor/notary-server-photon:v2.4.2
c635aace513a: Loading layer [==================================================>]    5.3MB/5.3MB
8deb84525956: Loading layer [==================================================>]   64.5MB/64.5MB
5d1431f9963f: Loading layer [==================================================>]  3.072kB/3.072kB
faeb0aac7135: Loading layer [==================================================>]  4.096kB/4.096kB
7ade25e3acdb: Loading layer [==================================================>]  65.29MB/65.29MB
Loaded image: goharbor/chartmuseum-photon:v2.4.2
27495e3181af: Loading layer [==================================================>]  7.222MB/7.222MB
88649dba6134: Loading layer [==================================================>]  7.356MB/7.356MB
875f964b6f85: Loading layer [==================================================>]  1.754MB/1.754MB
Loaded image: goharbor/harbor-portal:v2.4.2
c724cc796747: Loading layer [==================================================>]  124.3MB/124.3MB
b6c853c6dc0d: Loading layer [==================================================>]  3.584kB/3.584kB
0c5772798040: Loading layer [==================================================>]  3.072kB/3.072kB
c5f3bfcfa62d: Loading layer [==================================================>]   2.56kB/2.56kB
2602a8530e9d: Loading layer [==================================================>]  3.072kB/3.072kB
ba9b43b5ffb1: Loading layer [==================================================>]  3.584kB/3.584kB
cf92b578ba00: Loading layer [==================================================>]  20.48kB/20.48kB
Loaded image: goharbor/harbor-log:v2.4.2
fc1f8cdaf1ce: Loading layer [==================================================>]  1.096MB/1.096MB
eb68e72cbb03: Loading layer [==================================================>]  5.888MB/5.888MB
53a31c9e9836: Loading layer [==================================================>]  166.2MB/166.2MB
bed7172f8681: Loading layer [==================================================>]  17.75MB/17.75MB
c8fcd33ae148: Loading layer [==================================================>]  4.096kB/4.096kB
0c8d734a07ee: Loading layer [==================================================>]  6.144kB/6.144kB
c01db4825573: Loading layer [==================================================>]  3.072kB/3.072kB
4802d0abc8ba: Loading layer [==================================================>]  2.048kB/2.048kB
70bafeb87c65: Loading layer [==================================================>]   2.56kB/2.56kB
376ced88e40e: Loading layer [==================================================>]   2.56kB/2.56kB
0ba55f221469: Loading layer [==================================================>]   2.56kB/2.56kB
55cac263100c: Loading layer [==================================================>]  8.704kB/8.704kB
Loaded image: goharbor/harbor-db:v2.4.2
30ed654bbb0e: Loading layer [==================================================>]  5.301MB/5.301MB
51a81f0bf9ea: Loading layer [==================================================>]  4.096kB/4.096kB
12992ea6b45e: Loading layer [==================================================>]  3.072kB/3.072kB
91b97bcaa9d7: Loading layer [==================================================>]  17.32MB/17.32MB
1a992360f6fa: Loading layer [==================================================>]  18.12MB/18.12MB
Loaded image: goharbor/registry-photon:v2.4.2
d7224235291f: Loading layer [==================================================>]  5.301MB/5.301MB
dd26d9070a7b: Loading layer [==================================================>]  4.096kB/4.096kB
4022ecee5f13: Loading layer [==================================================>]  17.32MB/17.32MB
93d727d0accc: Loading layer [==================================================>]  3.072kB/3.072kB
8a56be8f0b84: Loading layer [==================================================>]  28.69MB/28.69MB
629472303402: Loading layer [==================================================>]  46.81MB/46.81MB
Loaded image: goharbor/harbor-registryctl:v2.4.2
bf4dfb7b7a70: Loading layer [==================================================>]  120.1MB/120.1MB
a0b0ce804b6b: Loading layer [==================================================>]  3.072kB/3.072kB
f088aae660dd: Loading layer [==================================================>]   59.9kB/59.9kB
2c15508db9bd: Loading layer [==================================================>]  61.95kB/61.95kB
Loaded image: goharbor/redis-photon:v2.4.2


[Step 3]: preparing environment ...

[Step 4]: preparing harbor configs ...
prepare base dir is set to /data/harbor
WARNING:root:WARNING: HTTP protocol is insecure. Harbor will deprecate http protocol in the future. Please make sure to upgrade to https
Generated configuration file: /config/portal/nginx.conf
Generated configuration file: /config/log/logrotate.conf
Generated configuration file: /config/log/rsyslog_docker.conf
Generated configuration file: /config/nginx/nginx.conf
Generated configuration file: /config/core/env
Generated configuration file: /config/core/app.conf
Generated configuration file: /config/registry/config.yml
Generated configuration file: /config/registryctl/env
Generated configuration file: /config/registryctl/config.yml
Generated configuration file: /config/db/env
Generated configuration file: /config/jobservice/env
Generated configuration file: /config/jobservice/config.yml
Generated and saved secret to file: /data/secret/keys/secretkey
Successfully called func: create_root_cert
Generated configuration file: /compose_location/docker-compose.yml
Clean up the input dir



[Step 5]: starting Harbor ...
Creating network "harbor_harbor" with the default driver
Creating harbor-log ... done
Creating harbor-portal ... done
Creating harbor-db     ... done
Creating redis         ... done
Creating registryctl   ... done
Creating registry      ... done
Creating harbor-core   ... done
Creating harbor-jobservice ... done
Creating nginx             ... done
✔ ----Harbor has been installed and started successfully.----
```



# 五，登陆

![harbor-1](D:\source\zhmingyong.github.io\images\harbor-1.png)



![](D:\source\zhmingyong.github.io\images\harbor-2.png)

# 六，其它命令

主要通过docker-compose管理的，所以通过docker-compose -h就可以了解harbor的日常启停命令。

# 七，更改管理员密码

1,进入harbor的数据库容器
`docker exec -it harbor-db /bin/bash`
2,进入postgres命令行



```css
psql -h postgresql -d postgres -U postgres  #这要输入默认密码：root123 。
psql -U postgres -d postgres -h 127.0.0.1 -p 5432  #或者用这个可以不输入密码。
```

3,切换到harbor所在的数据库
`\c registry`
4,查看harbor_user表
`select * from harbor_user;`
5,重置admin用户密码为Harbor12345
`update harbor_user set password='c999cbeae74a90282c8fa7c48894fb00', salt='nmgxu7a5ozddr0z6ov4k4f7dgnpbvqky' where username='admin';`
6,退出



```bash
\q     退出postgresql
exit  退出容器
```

7,接下来，就可以通过WEBUI登陆admin(admin/Harbor12345)用户，进行密码更新了。
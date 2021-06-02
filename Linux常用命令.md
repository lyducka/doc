### Linux常用命令

#### 关于授权chown和chmod

chown：用来改变目录或者文件的用户名和用户组

chmod：用来改变目录或者文件的访问权限

**修改用户组**

```
chown root:root test.txt #修改单个文件
chown -R root:root /www #修改某个目录并且遍历该目录
```

**修改文件权限**

```
u：代表文件所有者(user) 
g: 代表所有者所在的群组(group) 
o：代表其他人，但不是u和g(other)
a：a和一起指定ugo效果一样
```

```
chmod o+w test.txt ：表示给其他人授予写test.txt这个文件的权限
chmod go-rw test.txt : 表示群组和其他人删除对test.txt文件的读写权限
chmod ugo+r test.txt：所有人皆可读取
chmod a+r text.txt:所有人皆可读取
chmod ug+w,o-w text.txt:设为该档案拥有者，与其所属同一个群体者可写入，但其他以外的人则不可写入
chmod u+x test.txt: 创建者拥有执行权限 
chmod -R a+r ./www/ ：将www下的所有档案与子目录皆设为任何人可读取
chmod a-x test.txt :收回所有用户的对test.txt的执行权限
chmod 777 test.txt: 所有人可读，写，执行
```

**修改目录权限**

```
chmod 700 /opt/elasticsearch #修改目录权限 
chmod -R 744 /opt/elasticsearch #修改目目录以下所有的权限  
-R       # 以递归方式更改所有的文件及子目录
```

```
-rw------- (600) 只有所有者才有读和写的权限。 
-rw-r--r-- (644) 只有所有者才有读和写的权限，群组和其他人只有读的权限。 
-rw-rw-rw- (666) 每个人都有读写的权限 
-rwx------ (700) 只有所有者才有读，写和执行的权限。 
-rwx--x--x (711) 只有所有者才有读，写和执行的权限，群组和其他人只有执行的权限。 
-rwxr-xr-x (755) 只有所有者才有读，写，执行的权限，群组和其他人只有读和执行的权限。 
-rwxrwxrwx (777) 每个人都有读，写和执行的权限
```



#### Centos防火墙

**开放3306端口**

`firewall-cmd --zone=public --add-port=3306/tcp --permanent`

`firewall-cmd --zone=public --add-port=40000-45000/tcp --permanent` #开放一段端口

命令含义：

–zone #作用域

–add-port=80/tcp #添加端口，格式为：端口/通讯协议

–permanent #永久生效，没有此参数重启后失效

**查看开放的端口**

`firewall-cmd --list-ports`

**重启防火墙**

```
firewall-cmd --reload 
systemctl restart firewalld 
systemctl disable firewalld #禁止firewall开机启动
systemctl status firewalld #查看状态
```

**查看listen端口**

```
netstat -lnp #查看所有被监听端口
netstat -lnp|grep 3306 #查看3306被谁监听
```

#### 未分类

**Linux免密ssh登录**

`vim /root/.ssh/authorized_keys`

**frp system 启动**

```
mkdir -p /etc/frp
cp frps.ini /etc/frp
cp frps /usr/bin
cp systemd/frps.service /usr/lib/systemd/system/
systemctl enable frps
systemctl start frps
```

**vps测试**

`wget -qO- bench.sh | bash`

**解压tar.gz文件**

`tar -zxvf ×××.tar.gz`

**解压tar.bz2文件**

`tar -jxvf ×××.tar.bz2`

**清理登录日志btmp**

`echo '' > /var/log/btmp`

#### Screen 命令

`screen -S yourname` #新建一个叫yourname的session

`screen -ls` #列出当前所有的session

`screen -r yourname` #回到yourname这个session

`screen -d yourname` #远程detach某个session

`screen -d -r yourname` #结束当前session并回到yourname这个session

Ctrl+A+D 暂时离开session，不影响当前任务执行

#### 光标快速切换

ctrl+a #行首

ctrl+e #行尾

ctrl+k #删除至行尾

#### Bash历史记录快速补全

1.`vim ~/.bashrc`

```
#History search
if [[ $- == *i* ]]
then
        bind '"\e[A": history-search-backward'
        bind '"\e[B": history-search-forward'
        set show-all-if-ambiguous on
        set completion-ignore-case on
fi
```

注：`show-all-if-ambiguous` 是指tab补全时，按一次tab就会把最长匹配的自动补全

​		`completion-ignore-case` 是指tab补全时，忽略大小写。

2.`source ~/.bashrc`

#### acme.sh申请和nginx证书安装

**申请**

`./acme.sh --issue -d demo.notdnsapi.com --challenge-alias destination.xyz --dns dns_cf`

**nginx安装证书**

```
./acme.sh --install-cert -d down.dk.com \
 --key-file    /etc/nginx/ssl/down.dk.com.key \
 --fullchain-file /etc/nginx/ssl/down.dk.com.cer \
 --reloadcmd   "nginx -s reload"
```

#### Nginx相关

##### **Nginx加密访问**

**生成账号**

`htpasswd -c ./auth_conf user` #生成登录的账号密码

**nginx配置**

```
auth_basic "Auth access! Input your password!";
auth_basic_user_file /etc/nginx/auth_conf;
```

##### **Nginx配置跨域请求 Access-Control-Allow-Origin ***

只需要在Nginx的配置文件中配置以下参数：

```
location / {  
    add_header Access-Control-Allow-Origin *;
    add_header Access-Control-Allow-Methods 'GET, POST, OPTIONS';
    add_header Access-Control-Allow-Headers 'DNT,X-Mx-ReqToken,Keep-Alive,User-Agent,X-Requested-With,If-Modified-Since,Cache-Control,Content-Type,Authorization';

    if ($request_method = 'OPTIONS') {
        return 204;
    }
} 
```

##### Nginx文件目录浏览

放到location下面

```
autoindex on;  #开启文件目录浏览功能
autoindex_exact_size on;  #显示文件大小从KB显示
autoindex_localtime on;  #显示文件修改时间，为服务器本地时间
```


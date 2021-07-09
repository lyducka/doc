
       9  yum install nginx 
       10  systemctl enable nginx
       11  systemctl start nginx
       12  curl locahost
       13  yum install curl
       14  curl localhost
       15  ystemctl status nginx
       16  systemctl status nginx
       24  yum install -y yum-utils
       25  yum-config-manager --add-repo  http://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo 
       26  sudo yum install docker-ce docker-ce-cli containerd.io
       27  docker -v
       28  systemctl start docker
       29  docker ps -a
       30  systemctl enable docker
       31  docker pull centos:centos8
       32  docker images
       34  docker run -d -p 661:22 --name centos-python 300
       35  docker ps -a
       36  docker exec -it 1a7 /bin/bash
       37  docker rm 1a7
       38  docker run -d -p 661:22 --name centos-python 300 --privileged=true
       39  docker ps -a
       40  docker exec -it d61 /bin/bash
       41  docker stop d61
       42  docker rm d61
       43  docker run -it -d 300 /bin/bash
       44  docker ps -a
       45  docker exec -it 176 /bin/bash
       46  docker ps -a
       47  docker commit 1762123d1a05 centos8-ssh
       48  docker ps -a
       49  docker images
       50  docker stop 1762123d1a05
       51  docker run -d -p 661:22 cc739662b399 /usr/sbin/sshd -D
       52  docker ps -a
       53  docker rename 8f9 python
       54  docker ps -a
       55  docker run -d -p 662:22 cc739662b399 /usr/sbin/sshd -D
       56  docker ps -a
       57  docker rename 875 php
       58  docker ps -a
       59  docker pull mysql:5.7
       60  docker imiages
       61  docker images
       62  docker run -p 3306:3306 --name mysql -e MYSQL_ROOT_PASSWORD=12345 -d 5f47254ca
       63  docker ps -a
       64  docker inspect d611764165aa
       65  docker ps -a
       66  docker pull mongo:4.4
       67  docker images
       72  docker ps -a
       73  docker run -p 27017:27017 -d --name mongo -e MONGO_INITDB_ROOT_USERNAME=admin -e MONGO_INITDB_ROOT_PASSWORD=12345 b8264a857 --auth
       74  docker ps -a
       90  docker inspect php
       91  docker inspect python
      102  docker exec -it python /bin/bash
      110  docker pull ubuntu:20.04
      111  history 
      116  docker run -it -d f63181f19b2f
      117  docker ps -a
      120  docker exec -it 94554f04017a /bin/bash
      121  docker ps -a
      122  docker inspect 9455
      123  ssh root@172.17.0.2
      124  docker exec -it 94554f04017a /bin/bash
      125  ssh root@172.17.0.2
      126  docker ps -a
      127  history | grep commit
      128  docker commit 94554f04017a ubuntu-ssh
      129  docker images
      130  history 
      131  docker run -d -p 661:22 1152a1d8c3 /usr/sbin/sshd -D &
      132  docker ps -a
      133  docker rename ab6abc3dcb python
      134  ssh root@172.17.0.2
      137  docker inspect 83f17c88c69e
      138  exit
      139  docker ps -a
      140  netstat -ntlp
     
      
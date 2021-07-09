**1.启动初始docker**

`docker run -d -it f63181f19b2f /bin/bash`

**2.进入去配置环境**

`docker exec -it d55641612e44 /bin/bash`

上面两步还可以通过Dockerfile进行build

`docker build -t name:tag .`

**3.进行commit**

`docker commit d55641612e44 python:3.9.4`

**4.tag image**

`docker tag python:3.9.4 registry.cn-shanghai.aliyuncs.com/#NameSpace-zj/python:3.9.4`

**5.登录**

`docker login --username=name registry.cn-shanghai.aliyuncs.com`

**6.push**

`docker push registry.cn-shanghai.aliyuncs.com/#NameSpace-zj/python:3.9.4`

 


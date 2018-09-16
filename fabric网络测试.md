## 环境搭建步骤

##### 1、操作系统是Ubuntu16.04

##### 2、保证apt source是国内的

```javascript
执行如下命令：
$ sudo vi /etc/apt/sources.list
$ :%s/us./cn./g
以上命令将sources.list里面us开头的替换为cn开头，这样源就是国内的。最后wq保存并退出。
最后我们运行 sudo apt-get update更新下源
```

#####3、Go语言环境安装

```javascript
运行如下命令下载Go安装包：
	$ wget https://studygolang.com/dl/golang/go1.11.linux-amd64.tar.gz
解压
	$ sudo tar -C /usr/local -xvf go1.11.linux-amd64.tar.gz
接下来编辑环境变量运行如下命令(由于官方一般是/opt/gopath为GOPATH所以按照如下命令执行)：
	$ sudo mkdir -p /opt/gopath
	$ vi ~/.profile
在最后添加如下内容:
	export PATH=$PATH:/usr/local/go/bin 
	export GOROOT=/usr/local/go 
	export GOPATH=/opt/gopath 
	export PATH=$PATH:$HOME/go/bin
:wq保存并推出后运行如下命令使得环境变量生效：
	$ source ~/.profile
最后输入查看go版本:
	$ go version
输出如下结果表明go环境安装完成
	go version go1.11 linux/amd64
	
```

##### 4、docker 安装

```javascript
由于官方需要翻墙，所以将源改为aliyun，命令如下：
$ curl -sSL http://acs-public-mirror.oss-cn-hangzhou.aliyuncs.com/docker-engine/internet | sh -
然后修改当前用户的权限，以后方便操作，XXX为你的用户名，比如我的是tom：    
$ sudo usermod -aG docker XXX 
注销并重新登陆当前用户，然后添加阿里云的Docker Hub镜像，命令如下：    
$ sudo mkdir -p /etc/docker    
$ sudo tee /etc/docker/daemon.json <<-'EOF'    
{        
  "registry-mirrors": ["https://obou6wyb.mirror.aliyuncs.com"]	
}
EOF
$ sudo systemctl daemon-reload
$ sudo systemctl restart docker
最后运行如下命令，如果出现docker版本号正常，那么久安装完成:
$ docker version
输出如下:
	Client:
 Version:      17.05.0-ce
 API version:  1.29
 Go version:   go1.7.5
 Git commit:   89658be
 Built:        Thu May  4 22:10:54 2017
 OS/Arch:      linux/amd64

Server:
 Version:      17.05.0-ce
 API version:  1.29 (minimum version 1.12)
 Go version:   go1.7.5
 Git commit:   89658be
 Built:        Thu May  4 22:10:54 2017
 OS/Arch:      linux/amd64
 Experimental: false

```

##### 5、docker-compose安装

```javascript
Docker-compose是支持通过模板脚本批量创建Docker容器的一个组件。在安装Docker-Compose之前，需要安装Python-pip
命令如下:
	$ sudo apt-get install python-pip
然后安装docker-compose:
	$ curl -L https://get.daocloud.io/docker/compose/releases/download/1.12.0/docker-compose-uname -s-uname -m> ~/docker-compose
	$ sudo mv ~/docker-compose /usr/local/bin/docker-compose
	$ chmod +x /usr/local/bin/docker-compose
最后运行如下命令查看docker-compose版本，输出结果正常则安装完成：
	docker-compose version 1.17.0, build ac53b73
	docker-py version: 2.5.1
	CPython version: 2.7.13
	OpenSSL version: OpenSSL 1.0.1t  3 May 2016

```

##### 6、Fabric源码下载

```javascript
运行如下命令:
	$ mkdir -p $GOPATH/src/github.com/hyperledger
	$ cd $GOPATH/src/github.com/hyperledger
用git clone下载源码
	$ git clone https://github.com/hyperledger/fabric.git
上面代码是最新的代码，切换到v1.0.0能使用就ok：
	$ cd fabric/
    $ git checkout v1.0.0

```

##### 7、fabric 镜像下载

```javascript
运行如下指令:
	$ cd $GOPATH/src/github.com/hyperledger/fabric/examples/e2e_cli/
    $ source download-dockerimages.sh -c x86_64-1.0.0 -f x86_64-1.0.0
等待镜像下载完成运行 docker images查看镜像
运行如下命令启动fabric测试网络:
	$ ./network_setup.sh up
    最后看到输出all good就运行起来了测试网络。
```


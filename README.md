### docker compose 快速搭建PHP开发环境

* * *
##### 1、本地安装docker
Mac Docker 安装：https://www.runoob.com/docker/macos-docker-install.html
Docker 镜像加速：https://www.runoob.com/docker/docker-mirror-acceleration.html


```json
{
  "experimental": false,
  "debug": true,
  "registry-mirrors": [
    "https://7m85rdnb.mirror.aliyuncs.com",
    "http://hub-mirror.c.163.com",
    "https://registry.docker-cn.com"
  ]
}
```



##### 2、下载构建配置项目到本地
git clone https://github.com/1366101575/dcphp.git
PS：如果Git下载很慢，可通过 https://githubd.com 这个网站去代理下载



##### 3、进入构建配置项目根目录执行构建命令
docker-compose up
PS：如果centos7拉取很慢，可以先通过【 docker pull centos:7 】命令单独拉取centos镜像到本地再构建



##### 4、等待构建成功后，执行如下命令启动PHP容器中的qconf_agent
docker exec -it dcphp_php_1 bash -c "sh /usr/local/qconf/bin/agent-cmd.sh start"



##### 5、到此环境已构建完成，项目中用到本地MySQL，Redis，zookeeper的，修改相关项目配置文件中的host为容器的服务名即可

比如zookeeper的配置
`

"zk": [
	"test" => "zookeeper1:2181",
	"test2" => "zookeeper2:2181",
]

`

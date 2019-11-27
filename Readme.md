### 介绍
##### 此为开发测试用的单机Elasticsearch+Kibana+Logstash（以下简称ELK）环境版本，拥有3个es节点，Kibana平台，其中Elasticsearch已预先导入了ik中文分词及拼音分词扩展。

#### 警告：不可将此版本直接部署至生产环境

本镜像包含：
* elasticsearch(可自定义版本，推荐7以上)
* elasticsearch-ik/pingying 分词扩展（支持热更新词库）
* kibana(可自定义版本，推荐7以上)

#### 以下流程基于Centos7-minimal的系统进行部署，其他系统请参考。

### 安装Docker CE

* Docker CE Centos安装官方文档： https://docs.docker.com/install/linux/docker-ce/centos/
* Docker Compose安装官方文档： https://docs.docker.com/compose/install/
* Docker 国内镜像源： https://yeasy.gitbooks.io/docker_practice/content/install/mirror.html

### 部署ELK
* 建议创建uid为1000的用户
* git clone 此项目
* 将此项目的own改为1000的用户

####修改虚拟内存设置vm.max_map_count

切换到root账号修改配置`/etc/sysctl.conf`添加下面配置项。
```shell script
vm.max_map_count=262144
```

执行如下命令使配置生效。
```shell script
sysctl -p
```

####创建本机共享的docker网络层
```shell script
$ docker network create docker-custom-network
```

在项目docker-compose.yml文件所在目录，修改.env文件，并执行docker-compose
* 修改.env
* 先执行elasticsearch和kibana
```shell script
$ docker-compose up -d
```
涉及到多个下载安装过程，可能会消耗较久的时间

* 检查结果
```shell script
$ docker-compose ps
    Name                   Command               State                            Ports                          
-----------------------------------------------------------------------------------------------------------------
elasticsearch   /usr/local/bin/docker-entr ...   Up      0.0.0.0:9200->9200/tcp, 9300/tcp                        
es02            /usr/local/bin/docker-entr ...   Up      9200/tcp, 9300/tcp                                      
es03            /usr/local/bin/docker-entr ...   Up      9200/tcp, 9300/tcp                                      
kibana          /usr/local/bin/dumb-init - ...   Up      0.0.0.0:5601->5601/tcp                                  
```

* 检查elasticsearch启动情况
```shell script
curl 127.0.0.1:9200
```

* 检查结果
```shell script
{
  "name" : "es01",
  "cluster_name" : "docker-cluster",
  "cluster_uuid" : "xxx-_xxxxxxxxxxxxx",
  "version" : {
    "number" : "7.4.2",
    "build_flavor" : "default",
    "build_type" : "docker",
    "build_hash" : "xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx",
    "build_date" : "2019-10-28T20:40:44.881551Z",
    "build_snapshot" : false,
    "lucene_version" : "8.2.0",
    "minimum_wire_compatibility_version" : "6.8.0",
    "minimum_index_compatibility_version" : "6.0.0-beta1"
  },
  "tagline" : "You Know, for Search"
}
```

#### 参考网站
* Docker CE Centos安装官方文档： https://docs.docker.com/install/linux/docker-ce/centos/
* Docker Compose安装官方文档： https://docs.docker.com/compose/install/
* Docker 国内镜像源： https://yeasy.gitbooks.io/docker_practice/content/install/mirror.html 

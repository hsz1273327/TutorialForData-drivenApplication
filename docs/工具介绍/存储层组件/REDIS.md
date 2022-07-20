# REDIS

[redis](https://redis.io/docs)是一个基于内存的键值对型数据库,其特点也和内存一致

1. 快
2. 贵

## 评估

+ 通用性: 无可挑剔,几乎在在线部分全部都有应用场景
+ 接口稳定性: 无可挑剔,旧的数据结构一直非常稳定,新接口也相对稳定,redis的接口很少有改动,有也是增量式改动,不会影响使用旧接口的程序.
+ 编程语言支持: redis本身支持lua脚本自定义方法,客户端主流编程语言都有支持.
+ 可维护性: 提供多种部署方式, 配置项也说明比较清晰,官方文档几乎可以解决所有问题.

## 应用场景

因此通常redis只会用在最热和最常用的数据存储上,通常也不是离线使用而是在在线部分用于保存服务化所需的最常使用的数据.也就是说它一般会用在业务层和推理层,也有少部分会用在监控层

在应用层主要的应用场景包括:

1. 限流(使用插件[redis-cell@v0.3.0](https://github.com/brandur/redis-cell/tree/v0.3.0)),防止DDoS攻击造成应用瘫痪.
2. 缓存([RediSQL@v1.1.1_9b110f](https://github.com/RedBeardLab/rediSQL/tree/v1.1.1),[RedisJSON@v2.0.9](https://github.com/RedisJSON/RedisJSON/tree/v2.0.9)),一般用于缓存用户的访问会话信息以及一些常用数据,方便快速搜索,避免传导压力到业务数据库
3. 分布式锁(setex),一般使用分布式架构部署应用后台时可能会用到,避免连续请求异步任务
4. 分布式自增计数器(inc),一般用于生成分布式系统中的int64型的id
5. 布隆过滤器([RedisBloom@v2.2.15](https://github.com/RedisBloom/RedisBloom/tree/ver2.2.15)),一般用于一些内容服务的去重业务,以及签到等场景
6. 作为队列或者pubsub中间件,这种一般是小规模用,而且只对实时性要求高,允许数据丢失,比如用在一些推送场景

在推理层主要的应用场景包括:

1. 热数据常用数据存储,例如特征管理工具[feast](https://github.com/feast-dev/feast)的在线特征部分只能使用redis.
2. 布隆过滤器([RedisBloom@v2.2.15](https://github.com/RedisBloom/RedisBloom/tree/ver2.2.15)),比较常见的是推荐系统中用于去重.
3. 作为队列或者pubsub中间件,这种一般是小规模用,而且只对实时性要求高,允许数据丢失,比如在交易系统中作为触发信号

在监控层主要应用场景包括:

1. 计数器(hyperloglog),粗略统计监控日活等数据

## 部署形式

redis支持的部署方式包括:

1. 单机模式,最简单的模式,一般用在单一用途且一旦崩溃对于业务不会有过大影响的情况下.比如作为缓存,比如作为限流器
2. 主备模式,结构最简单的分布式部署方式,可以一主多备,可以分层备份,需要注意redis的主备部署使用异步复制,因此并不能保证数据完全不会丢失.主要用于:
    1. 数据冗余
    2. 读写分离,提升扩展性(scalability)
    只要在从redis上启动服务时使用`--replicaof hostname port`指定要备份的主机即可.如果要设置从服务器能不能执行写操作(默认行为是不能写),也可以简单的使用`--replica-read-only yes`设置
3. 哨兵模式,主备部署的扩展,主要是解决redis的高可用问题,其原理是使用复数个哨兵服务监控主备部署的redis,当主机崩溃时哨兵会表决提升备机为新的主机.主要用于:
    1. 对高可用有较高要求,但并不需要集群这么重的方案的场景
    要启动哨兵需要额外配置一个配置文件`sentinel.conf`,并使用命令`redis-server sentinel.conf --sentinel`启动redis服务.这个`sentinel.conf`的样版如下

    ```txt
    sentinel monitor mymaster redis-inone-master 6379 2 //指定内容语法为 sentinel monitor 主机名 主机hostname 主机port 最少表决通过数
    sentinel down-after-milliseconds mymaster 60000
    sentinel failover-timeout mymaster 180000
    sentinel parallel-syncs mymaster 1

    sentinel resolve-hostnames yes //sentinel monitor中设置为可以用hostname替代ip
    ```

    当启动后`sentinel.conf`会被服务修改,用于记录监控状况

4. 集群模式,高扩展高可用的redis部署模式,一般用在部署关键应用上,确保redis不会无效.
    集群模式的原理实际是hash分片和主备部署的结合.集群中并不是所有节点都有相同的地位.所有节点可以分为主节点和备节点,主节点和备节点依然是一主多备的对应关系.集群模式只是提供了自带的哨兵以及一个hash分片功能将key根据hash后的值散列到不同的主节点上.具体的算法为计算key的CRC16结果再模16834.这16834个位置被称为哈希槽,集群分片说白了就是在分配哈希槽的落地位置.
    集群模式的部署步骤如下:
    1. 增加如下配置平行的启动多个redis服务:

        ```txt
        cluster-enabled yes
        cluster-config-file nodes.conf
        cluster-node-timeout 5000
        ```

        注意redis集群模式除了服务端口(默认6379,可以通过`port`配置)外还会通过总线端口(默认16379,可以通过`cluster-port`配置)在集群节点间通信.

    2. 随便连一个redis服务,执行命令`redis-cli --cluster create ip1:port1 ip2:port2 ip3:port3 ip4:port4 ip5:port5 ip6:port6 ... --cluster-replicas 1`,这会将部署的redis服务串起来,并且以每个主节点配一个备节点的方式构造一个redis集群.需要注意目前redis集群只能通过ip连接而不能使用hostname,因此如果是docker部署我们需要为容器分配静态ip,如果是docker swarm部署我们就只能用host模式部署了

    我们可以使用命令`cluster nodes`查看集群的状态,其中每一行的第一列就是节点的id,节点

    如果要扩展集群(增加节点)我们要分两种情况:
    + 如果要添加的是主节点:
        1. `redis-cli --cluster add-node ipnew:portnew  ipexist:portexist`将节点添加进集群
        2. `redis-cli --cluster reshard ipexist:portexist`为集群重新分配哈希槽.它会询问我们要移动多少哈希槽,要移动给那个目标,从哪里获得这些哈希槽,我们可以先后填上哈希槽数目,目标id,然后填all,这样就会从所有节点中凑满要移动的数目进行重新分配

    + 如果要添加的是从节点有两种方法:
        1. `redis-cli --cluster add-node ipnew:portnew  ipexist:portexist  --cluster-slave`将节点作为备节点随机分配一个主节点挂载.`redis-cli --cluster add-node ipnew:portnew  ipexist:portexist  --cluster-slave --cluster-master-id 主机id`也可以指定要挂载的主节点
        2. `redis-cli --cluster add-node ipnew:portnew  ipexist:portexist`将节点添加进集群,之后进入新添加的节点,执行`cluster replicate 主机id`.

    如果要收缩集群(删除节点),我们依然是分两种情况:
    + 要删除的是从节点,直接使用`redis-cli --cluster del-node ipexist:portexist 主机id`
    + 要删除的是主节点,先执行`redis-cli --cluster reshard ipexist:portexist`将要删除的节点上的哈希槽清空,然后执行`redis-cli --cluster del-node ipexist:portexist 主机id`删除节点

如果条件允许,我当然更加推荐使用redis集群,但部署集群很吃资源,在多数情况下其实单机模式已经很够.

## 使用逻辑

redis是键值对型数据库,本质上和哈希表(python中对应字典)类似,我们通过键获取其中保存的值.键可以搜索可以遍历,值则有多中数据类型,不同的数据类型有不同的性质和用法,具体则需要查官方文档.

redis的键除了可以有值设置外,还可以设置过期,这是redis的一个重要特性,键过期后redis会删除该键,具体的删除触发点有两种--数据固话到硬盘时和下次被检索时.

## 使用注意事项

1. 应该尽量为每个key设置过期时间,
2. 不要用阻塞的`keys`命令搜索键,而该用非阻塞的`scan`
3. 键的命名应该有规则,通常我们用命名空间的形式给键命名,即`A-a:C-b:E-e`.属性和属性值间用`-`隔开,属性和属性间用`:`隔开,属性如果是多个单词组成单词间用`_`隔开,属性键用纯大写,属性值用纯小写,属性间顺序按字符ascii表顺序排序
4. 键不应过长,键的最大长度为512M,但到达1KB就会影响搜索效率
5. 值最大也是512M,但应该尽量避免大值,除非信息密度很大

## 相关工具和文章推荐

1. 我自己构造了项目<https://github.com/Basic-Components/redis-allinone>打包了一个redis的通用镜像,用里面的docker-compose文件可以方便的构造各种模式的redis服务用于调试
2. 我自己封装了项目<https://github.com/Golang-Tools/redishelper>提供了redis比较常用的用法的封装.
3. 我的文章[常见的消息中间件模式](https://blog.hszofficial.site/experiment/2019/04/09/%E5%B8%B8%E8%A7%81%E7%9A%84%E6%B6%88%E6%81%AF%E4%B8%AD%E9%97%B4%E4%BB%B6%E6%A8%A1%E5%BC%8F/)中对redis作为消息中间件的用法有较详细的描述.
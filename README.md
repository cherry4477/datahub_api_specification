## DataHub API简介 ##

**DataHub**是一个数据P2P流动的平台，详情请见hub.dataos.io.
本文规定了包括认证、版本控制、流量控制、协议内容等信息。



### 版本控制 ###
在请求的消息头中明确API的版本。

**例如:**
**X-DataHub-Version: 1**

### 流量控制 ###
在返回的消息头中给出流量控制参数.

**周期内还能调用的次数:X-RateLimit-Remaining: 1234**

**周期重置时间：X-RateLimit-Reset: 1350085300**

### 返回信息 ###
HTTP状态码

200 OK (数据体中code=0)

40X Errors (用户错误，具体在数据体中进行说明,为用户展示，code非0) 

50X Internal Error （系统错误，具体在数据体中说明，不为用户展示，code非0）

数据体返回格式：

{

    "code": 0,
    "msg": "OK",
    "data": {具体返回信息}
}

code 及 msg 
	
	0				OK								            成功
	1001			unknown error					            未知错误
	1002			json building error				            json解释失败
	1003			unsupported url				    	        不支持的URL
	1004			db is not inited    				        DB没有初始化
	1005		    unauthorized			    		        认证失败
	1006			permission denied		    		        没有权限
	1007			invalid parameters			    	        参数违法
	1008			database operate		    		        数据库操作
	1009			xxx yyy not found.			    	        xxx yyy 没有找到
	1010			file operation				    	        文件操作失败
    1011	        dataitem out of limit 50                    dataitem数量超出
    1012	        repository out of limit                     创建rep失败,没有配额
    1013	        dataitem price out of limit 6"              dataitem 价格计划数量超过6
    1014	        repository exists cooperate dataitem"       rep下存在协作者dataitem
    1015	        no login                                    未登录
    1015            repository exists dataitem, can not delete  rep下存在dataitem
	1400			no parameter		    			        参数缺失
	1011			server link error				            连接其他服务错误
    6001            empty user is not allowed                   用户名不能为空
    6002            user not registered yet                     用户未注册
    6003            user doesn't have any entrypoint            用户没有Entrypoint
    6101            missing authorization info                  缺少验证信息
    6201            daemonid is empty                           缺少Daemonid
    6202            entrypoint not available                    EntryPoint不可达
    8001		    no quota info			                    没有该用户配额信息
    8002		    user exist			                        注册用户已存在
    8004		    pwd wrong			                        原始密码错误
    8005		    no login			                        未登录
    8006		    quota exist			                        用户配额信息已存在
    8007		    Sorry, your credit is running low           余额不足
    8008		    duplicate order requset		                订单请求重复提交
    8009		    order is not exist		                    订单不存在
    8010		    Illegal request			                    非法请求


### 认证方式 ###
####Basic认证模式通过用户名和md5(密码获取token)，访问的url为/####
#####请求报文的header#####

```
Authorization: Basic akmklmasadalkm==
```
#####正常情况下返回#####
```
HTTP/1.1 200 OK
Server: openresty/1.9.3.1
Date: Fri, 08 Jan 2016 05:53:24 GMT
Content-Type: application/json
Transfer-Encoding: chunked
Connection: keep-alive

{"code": 0,"msg": "OK","data": {"token": "ef47b6d4670b90eb3cf75a39f0854b0a"}}
```

#####用户没有激活#####
```
HTTP/1.1 403 Forbidden
Server: openresty/1.9.3.1
Date: Fri, 08 Jan 2016 05:53:31 GMT
Content-Type: application/json
Transfer-Encoding: chunked
Connection: keep-alive

{"code": 1102,"msg": "user inactive","data": {}}
```

#####密码错误5次内返回#####
```
HTTP/1.1 403 Forbidden
Server: openresty/1.9.3.1
Date: Fri, 08 Jan 2016 05:53:31 GMT
Content-Type: application/json
Transfer-Encoding: chunked
Connection: keep-alive

{"code": 1101,"msg": "username or password not correct","data": {"retry_times": "2","ttl_times":"86400"}}
```
#####密码错误5次以后，账户被锁定24小时#####
```
HTTP/1.1 403 Forbidden
Server: openresty/1.9.3.1
Date: Fri, 08 Jan 2016 05:53:35 GMT
Content-Type: application/json
Transfer-Encoding: chunked
Connection: keep-alive

{"code": 1103,"msg": "retry too many times!!","data": {"retry_times": "5","ttl_times":"86399"}}
```
####Token认证模式利用上一步获取的token来对需要认证的API提交####
#####请求报文的header#####
```Authorization: Token xa12344a```


####微服务组件校验Token ####
请求报文

	GET /valid
	Authorization: Token xa12344a
	Authuser: xxx@aaa.com

正确回复

```
HTTP/1.1 200 OK

{"code": 0,"msg": "OK","data": {}}
```
错误回复

```
HTTP/1.1 403 OK

{"code": 1403,"msg": "not valid","data": {}}
```

## DataHub API内容 ##
### repositories ###
编号：1

名称：数据中心API

描述文件：repositories.md

**编号10 [get /repositories](/repositories.md/)** 【拥有者|任意】[auth] **这是一个特殊需求**如果后面没有username的参数，则返回本人创建或具备写权限的所有repository，如果带有参数?username=XXX，则表示查询XXX的所有repository，这里只能反馈查询者具备读权限的repositories名字（比如public或者在private白名单中的）

**编号11 [get /repositories/:repname](/repositories.md/)** 【任意】返回repository详细信息

**编号12 [post /repositories/:repname](/repositories.md/)** 【任意】[auth]创建repository

**编号13 [delete /repositories/:repname](/repositories.md/)** 【拥有者】[auth]删除repository

**编号14 [get /repositories/:repname/:itemname](/repositories.md/)** 【任意】返回此DataItem的详细情况

**编号15 [post /repositories/:repname/:itemname](/repositories.md/)** 【拥有者】[auth]发布DataItem

**编号16 [delete /repositories/:repname/:itemname](/repositories.md/)** 【拥有者】[auth]删除DataItem

**编号17 [get /repositories/:repname/:itemname/:tag](/repositories.md/)** 【任意】查询DataItem下的Tag详情，**注意Tag需要按照UTF8编码后传递**

**编号18 [post /repositories/:repname/:itemname/:tag](/repositories.md/)** 【拥有者】[auth]发布DataItem下的Tag详情，**注意Tag需要按照UTF8编码后传递**

**编号19 [delete /repositories/:repname/:itemname/:tag](/repositories.md/)** 【拥有者】[auth]删除DataItem下的Tag详情，**注意Tag需要按照UTF8编码后传递**


编号：1a

名称： Label

描述文件：label.md

**编号1a1 [delete /label/:repname](/label.md/)**[auth]删除Repositories label

**编号1a2 [get  /label/:repname](/label.md/)**[auth]更新Repositories label

**编号1a3 [put  /label/:repname](/label.md/)**查询Repositories label

**编号1a4 [delete /label/:repname/:itemname](/label.md/)**[auth]删除Dataitem label

**编号1a5 [get /label/:repname/:itemname](/label.md/)**[auth]更新Dataitem label

**编号1a6 [put /label/:repname/:itemname](/label.md/)**查询Dataitem label

编号：1b

名称： permission

描述文件：permission.md
	
**编号1b1 [get 	/permission/:repname](/permission.md/)**[auth]repository相关权限查询

**编号1b2 [put 	/permission/:repname](/permission.md/)**[auth]repository相关权限增加

**编号1b3 [delete	/permission/:repname](/permission.md/)**[auth]repository相关权限删除

**编号1b4 [get  /permission/:repname/:itemname](/permission.md/)**[auth]dataitem相关权限查询    

**编号1b5 [put  /permission/:repname/:itemname](/permission.md/)**[auth]dataitem相关权限增加  

**编号1b6 [delete  /permission/:repname/:itemname](/permission.md/)**[auth]	dataitem相关权限删除  
   
### selects ###
编号：2

名称：精选API

描述文件：selects.md

**编号20 [get /select_labels](/selects.md/)** 【任意】返回精选栏目

**编号21 [post /select_labels/:labelname](/selects.md/)** 【管理员】[auth]创建精选栏目

**编号22 [put /select_labels/:labelname](/selects.md/)** 【管理员】[auth]更新精选栏目

**编号23 [delete /select_labels/:labelname](/selects.md/)** 【管理员】[auth]删除精选栏目

**编号24 [get /selects](/selects.md/)** 【任意】返回精选内容

**编号25 [post /selects](/selects.md/)** 【管理员】[auth]创建精选内容

**编号26 [put /selects](/selects.md/)** 【管理员】[auth]更新精选内容

**编号27 [delete /selects](/selects.md/)** 【管理员】[auth]删除精选内容

### search ###
编号：3

名称：搜索API

描述文件：search.md

**编号30 get /search** 【任意】搜索


### subscriptions ###
编号：4

名称：订阅API

描述文件：subscriptions.md

**编号40 [get /subscriptions?asconsumer=[0|1]&repname={repname}&itemname={itemname}](/subscription.md/)** 【需求者】[auth]查询所有订阅的DataItem 【提供者】查询所有被被人订阅的DataItem

**编号42 [post /subscription/:repname/:itemname](/subscription.md/)** 【需求者】[auth] 订阅该DataItem **注意没有s**

**编号44 [get /subscription_stat/:repname/:itemname](/subscription.md/)** 【任意】返回该DataItem的订阅量

**编号45 [get /subscription_stat/:repname](/subscription.md/)** 【任意】返回该repositories的订阅量

**编号47 [put /subscription/:subid](/subscription.md/)** 【需求者】取消订阅cancel 【提供者】投诉订阅flag 【管理员】取消订阅remove

### transactions ###
编号：5

名称：交易API

描述文件：transactions.md

**编号50 [post /transaction/:repname/:itemname/:tag](/subscription.md/)** 【需求者】[auth] 返回该请求的access_token，**注意tag需要UTF8编码，注意后面没有s**

**编号51 [get /transaction/:repname/:itemname/:tag?cypt_accesstoken=???&username=???](/subscription.md/)** 【拥有者】校验该access_token的有效性，此access_token被需求者的私有证书加密，注意后面没有s

**编号52 [get /transactions_stat/:repname/:itemname](/subscription.md/)** 【任意 | auth】返回该DataItem的pull量

**编号53 [get /transactions_stat/:repname](/subscription.md/)** 【任意 | auth】返回该repositories的pull量

**编号54 [get /transactions_stat/:repname/:itemname/:tag](/subscription.md/)** 【任意 | auth】返回该tag的pull量

**编号55 [get /transactions/pull](/subscription.md/)** 【需求者】[auth] 返回该用户所有的pull的信息

**编号56 [get /transactions/pull/:repname/:itemname](/subscription.md/)** 【需求者】[auth] 返回该用户pull的该DataItem的tag信息

**编号57 [get /transactions/push](/subscription.md/)** 【拥有者】[auth] 返回该用户所有的被人家Pull的信息

**编号58 [get /transactions/push/:repname/:itemname](/subscription.md/)** 【拥有者】[auth] 返回该用户所有的被人家Pull的该DataItem的tag信息

### heartbeats ###
编号：6

名称：心跳API

描述文件：heartbeat.md

**编号61 [post /heartbeat](/heartbeat.md#指令post-heartbeat-发送心跳信息)** 【Daemon】心跳信息由Daemon根据配置的心跳周期发送给Server，作用之一为新Daemon上线后向Server注册；二为汇报Daemon的健康情况，上报自己的entrypoint；三为上传daemon的日志。

### daemon ###
编号：7

名称：daemonAPI

描述文件：daemon.md

**编号71 [get /daemon/ep/:user](/daemon.md#指令get-daemonepuser-获取user的入口地址)** 【Server】返回作为数据提供方user的入口地址

**编号72 [get /daemon/id](/daemon.md#指令get-daemonid-获取user的daemonid)** 【Server】 给用户分配一个唯一标识并返回，用于在用户安装Daemon并启动时向server表明自己的身份

**编号73 [get /daemon/log/:index](/daemon.md#指令get-daemonlogindex-获取用户的daemon日志)** 【Server】 返回以index为起始索引的用户daemon日志，索引范围为[index, index+9]。

### users ###
编号：8

名称：用户API

描述文件：users.md

**编号81 [GET /users/:loginname](/user.md/) **查询用户

**编号82 [POST /users/:loginname](/user.md/) ** 创建用户

**编号83 [PUT /users/:loginname](/user.md/) ** 修改用户

**编号84 [DELETE /users/:loginname](/user.md/) ** 删除用户

**编号85 [PUT /users/:loginname/status](/user.md/) ** 激活用户

**编号86 [PUT /users/:loginname/pwd](/user.md/) ** 修改密码

**编号87 [GET /users/:loginname/repository](/user.md/) ** 获取repo配额信息

**编号88 [POST /users/:loginname/repository](/user.md/) ** 创建repo配额信息

**编号89 [PUT /users/:loginname/repository/quota](/user.md/) ** 修改repo配额

**编号8a [POST /users/:loginname/repository/use](/user.md/) ** 修改repo的使用量

**编号8b [GET /users/:loginname/deposit](/user.md/) ** 获取托管配额信息

**编号8c [POST /users/:loginname/deposit](/user.md/) ** 新建托管配额

**编号8d [PUT /users/:loginname/deposit/quota](/user.md/) ** 修改用户的托管配额

**编号8e [GET /users/:loginname/pullnum](/user.md/) ** 获取用户下载量配额信息

**编号8f [POST /users/:loginname/pullnum/](/user.md/) ** 创建用户下载量配额

**编号8g [PUT /users/:loginname/pullnum/quota](/user.md/) ** 修改用户下载量配额

**编号8h [PUT /users/:loginname/pullnum/use](/user.md/) ** 修改用户的已下载量

### messages (notifications) ###
编号：9

名称：消息通知API

描述文件：messages.md

**编号90 [post /notifications](/messages.md/)** 【用户】[auth] 创建一条用户提醒

**编号91 [get /notifications?type={type}&sender={sender}&status={status}&beforetime={beforetime}](/messages.md/)** 【用户】[auth] 返回当前用户接受到的消息列表 

**编号92 [get /notification_stat](/messages.md/)** 【用户】[auth] 取得自己的提醒汇总统计

**编号93 [delete /notification_stat](/messages.md/)** 【用户】[auth] 清除自己的提醒汇总统计

### star ###
编号：A

名称：点赞API

描述文件：star.md

**编号A0 [get /star/:repname/:itemname](/star.md/)** 【用户】[auth]查询是否star了某个DataItem

**编号A1 [put /star/:repname/:itemname](/star.md/)** 【用户】[auth] 更改对一个DataItem的star状态

**编号A2 [get /star_stat/:repname/:itemname](/star.md/)** 【任意】查询某个DataItem的点赞量 

**编号A3 [get //star_stat/:repname](/star.md/)** 【任意】查询某个Repository的点赞量 

### comment ###
编号：B

名称：评论留言API

描述文件：comment.md

**编号B0 [post /comment/:repname/:itemname](/comment.md/)** 【用户】对一个dataitem发表一个评论 (欲评论，先订阅)

**编号B1 [put /comment/:repname/:itemname](/comment.md/)** 【用户】修改自己对一个dataitem的评论

**编号B2 [delete /comment/:repname/:itemname?commentid={commentid}](/comment.md/)** 【用户】删除自己对一个dataitem的评论 

**编号B3 [get /comments/:repname/:itemname?beforetime={beforetime}](/comment.md/)** 【任何】返回一个dataitem上的评论列表

**编号B4 [get /comment_stat/:repname/:itemname](/comment.md/)** 用户】【任何】返回一个dataitem上的评论数 

### vip ###
编号：C

名称：会员管理

描述文件：vip.md

**编号C1 [GET /vip/:loginname](/vip.md/)** 查看用户会员相关信息

**编号C1 [POST /vip/:loginname](/vip.md/)** 添加用户会员相关信息

**编号C1 [PUT /vip/:loginname](/vip.md/)** 修改用户会员相关信息


## DataHub API应用 ##

### WEB ###
**数据精选** 20，24，14，44，52

**Item详情** 14，17,41，56

**Repo详情** 11,44,45,52,53

**数据拥有者详情** 7 10

**搜索页面** 30,14,44,52

**我的发布** 10,14,44,52

**我发布的Repo详情** 11,44,45,52,53

**我发布的Item详情** 14,17,44,52,54

**我的订阅** 40，14，（41），52

**我的流水**  55或57

### Daemon ###

**repo**  10

**repo create**  12

**repo rm**  13

**subs**  40

**subs :repo/:item**  14

**pub :repo/:item**  15

**pub :repo/:item/:tag**  18

**pull :repo/:item/:tag**  50或51

**login**  7


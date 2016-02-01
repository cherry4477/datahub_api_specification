# Messages

### POST /notifications

说明

	【管理员】给用户发消息 / 发站内广播消息

输入参数说明：	
	
	type: 消息类型，必选
	receiver:接收用户列表或者*(表示群发)
	data: （不同type有各自不同的data）
	
输入样例(管理员给用户发消息，最多同时100个用户)：

	POST /notifications HTTP/1.1
	Accept: application/json
	Authorization: Token dcabfefb6ad8feb68e6fbce876fbfe778fb
	
	{
		"type": "admin_message",
		"receiver": "zhang3@example.com,li4@example.com,john@example.com",
		"data": {
			"content": "bla bla ..."
		}
	}

输出样例：

	空

### GET /notifications?forclient={forclient}&&type={type}&sender={sender}&status={status}&level={level}&page={page}&size={size}

说明

	【用户】返回当前用户接受到的消息列表

输入参数说明：
	
	type: （可选）消息类型
	sender: (可选) 消息发送者
	status: (可选, 默认为2) 0: 未读, 1: 已读, 2: either
	level: (可选) 0: 普通消息，50: 需用户进一步确认的消息。如果不指定，则表示所有等级的消息。
	page: (可选) 第几页，最小值为1。
	size: (可选) 每页最多返回多少条数据
	forclient: （可选，默认为0），是否返回浏览器或者客户端感兴趣的消息。(0: 浏览器感兴趣的消息；1: 客户端感兴趣的消息)

输入样例：

	GET /notifications HTTP/1.1
	Accept: application/json
	Authorization: Token dcabfefb6ad8feb68e6fbce876fbfe778fb

输出样例：

	{
		"total": 100,
		"results": [
			{
				"messageid": 12,
				"type": "item_event",
				"level": 0,
				"status": 1,
				"time": "2015-11-10T15:04:09Z08:00",
				"data": {
					"event": "tag_added",
					"eventtime": "2015-11-10T15:04:09Z08:00",
					"repname": "repo001",
					"itemname": "item123",
					"tag": "tag567"
				}
			},
			{
				"messageid": 19,
				"type": "subsapply_event",
				"level": 50,
				"status": 0,
				"time": "2015-11-10T15:06:09Z08:00",
				"data": {
					"subscriptionid": 1234567,
					"sellername": "li4@example.com"
					"repname":"NBA",
					"itemname":"bear",
					"supply_style":"batch",
					"applytime":"2015-11-10T15:04:05Z08:00",
					"expiretime":"2016-11-17T15:04:05Z08:00",
					"phase":7,
					"plan":{
						"money":5,
						"units":3,
						"used":0,
						"limit":0,
						"subs":1,
						"expire":30
					}
				}
			}
		]
	}

### GET /notification_stat

说明

	【用户】 取得自己的提醒汇总统计

输入参数说明：	
	
	无

输入样例：

	GET /notification_stat HTTP/1.1
	Accept: application/json
	Authorization: Token dcabfefb6ad8feb68e6fbce876fbfe778fb

输出样例：

	{
		"subsapply_event": 6,
		"item_event": 20,
		"vip_remind": 1,
		"admin_message": 1,
		"comment_reply": 2
	}
	
输出样例说明：

	subsapply_event: 订购申请事件
	item_event: data item事件
	vip_remind: 会员续费提醒
	admin_message: 管理员消息
	comment_reply: 留言被回复消息

### DELETE /notification_stat

说明

	【用户】清除自己的提醒汇总统计

输入参数说明：	
	
	无

输入样例：

	DELETE /notification_stat HTTP/1.1
	Accept: application/json
	Authorization: Token dcabfefb6ad8feb68e6fbce876fbfe778fb

输出样例：

	空

# Messages Lib

## 初始化(golang)

```go
	import (
		"github.com/asiainfoLDP/datahub_messages/mq"
	)

	var (
		theMQ mq.MessageQueue
	)
	
	func initMQ() {
		kafkas := fmt.Sprintf("%s:%s", os.Getenv("KAFKA_HOST"), os.Getenv("KAFKA_PORT"))
		log.Infof("kafkas = %s", kafkas)
		theMQ, err := mq.NewMQ([]string{kafkas}) // ex. {"192.168.1.1:9092", "192.168.1.2:9092"}
		if err != nil {
			log.Infof("initMQ error: %s", err.Error())
			return
		}
		
		// if this service will call other service with http format, call this line.
		// "datahub_messages_caller" is just an example topic. 
		// You should use a unique topic name for your service module.
		theMQ.EnableApiCalling("datahub_messages_caller")
		
		// if this service will be called by other services with http format, call this line.
		// "datahub_messages_handler" is just an example topic. 
		// You should use a unique topic name for your service module.
		theMQ.EnableApiHandling(Port, "datahub_messages_handler", mq.Offset_Marked) // or Offset_Newest, Offset_Oldest
	}
```

## 消息发送

目前消息支持http和自定义格式。每种格式都支持同步和异步发送。但是同步的意义对于http和自定义格式有所不同。对于http发送，同步意味着发送者将得到一个消息接受者的回应。对于自定义格式，同步仅仅意味着发送者得到了消息队列服务器的回应(成功或不成功)。

在以下示例中，[]byte("")表示messsage key，在多数情况下，它可以被忽略。
	
### 自定义格式同步发送
	
```go
	partition, offset, err := theMQ.SendSyncMessage("a_topic_for_others_to_read", []byte(""), []byte("hello world")
```

### 自定义格式异步发送
	
```go
	err := theMQ.SendAsyncMessage("a_topic_for_others_to_read", []byte(""), []byte("hello world")
```

### http同步发送

```go
	// here localhost is just a placeholder, whatever it is.
	request, err := http.NewRequest("GET", "http://localhost/subscription_stat/a/b", nil)
	if err != nil {
		log.Debugf("create request error: %s", err.Error())
		return
	}
	
	response, err := theMQ.SyncApiRequest("datahub_subscription_handler", []byte(""), request)
	if err != nil {
		log.Debugf("response error: %s", err.Error())
		return
	}
	
	log.Debugf("response.Status = %s", response.Status)
	
	// ...
```

### http异步发送

```go
	// here localhost is just a placeholder, whatever it is.
	request, err := http.NewRequest("GET", "http://localhost/subscription_stat/a/b", nil)
	if err != nil {
		log.Debugf("create request error: %s", err.Error())
		return
	}

	err = theMQ.AsyncApiRequest("datahub_subscription_handler", []byte(""), request)
	if err != nil {
		log.Debugf("AsyncApiRequest: %s", err.Error())
		return
	}
```

## 消息接收

欲接收消息，需实现如下mq.MassageListener接口

```go
	type MassageListener interface {
		// return whether or not the offset will be marked in server
		OnMessage(key, value []byte, topic string, partition int32, offset int64) bool
		
		// return whether or not to stop listenning 
		OnError(error) bool
	}
```

Example:
	
```go
	type MyMesssageListener struct {
		name string
	}
	
	func newMyMesssageListener(name string) *MyMesssageListener {
		return &MyMesssageListener{name: name}
	}
	
	func (listener *MyMesssageListener) OnMessage(key, value []byte, topic string, partition int32, offset int64) bool {
		log.Debugf("%s received: (%d) message: %s", listener.name, offset, string(value))
		return true // to save offset on server
	}
	
	func (listener *MyMesssageListener) OnError(err error) bool {
		log.Debugf("api response listener error: %s", err.Error())
		return false // will not stop listenning
	}
```
	
然后注册此MassageListener

```go
	myListener := newMyMesssageListener("test")
	
	// 0 is the partition id
	err := theMQ.SetMessageListener("the_topic_i_will_read", 0, mq.Offset_Marked, myListener)
	if err != nil {
		// ...
	}
```

# json消息格式 (目前在使用)

json消息格式被视为自定义格式。json将被转化为bytes进行传输。

## topic: to_notifications.json

当发送消息至to_notifications.json topic时，可以在key中加入特定字符串暗示此消息是否是一个前端消息或者是一个客户端消息。
当key中包含forclient字段时，此消息将被存入MessageTabel_ForClient。当key中不包含notforbrowser字段时，此消息将被存入MessageTable_ForBorser。

当发送一个message时，可以带一个可选level字段(默认为0)，表示重要度。

	level=0: general
	level=50: 需要用户进一步处理

### 管理员消息
	
	{
		"type": "admin_message",
		"receiver": "zhang3@example.com",
		"sender": "admin@hub.dataio.com",
		"time": "2015-11-10T15:06:09Z08:00",
		"data": {
			"content": "bla bla ..."
		}
	}
	
### comment reply
	
	{
		"type": "comment_reply",
		"receiver": "zhang3@example.com",
		"sender": "",
		"level": 0,
		"time": "2015-11-10T15:06:09Z08:00",
		"data": {
			"commentid": 1234567,
			"repname": "repo00", 
			"itemname": "item01",
			"username": "li4@bbb.com",
			"nickname", "小李",
			"createtime": "2015-11-10T15:04:05Z08:00",
			"content": "agree!",
			"replyto": {
				"commentid": 1234560,
				"content": "cool data! ..."
			}
		}
	}

### dataitem events

	{
		"type": "item_event",
		"receiver": "zhang3@example.com",
		"sender": "",
		"time": "2015-11-10T15:06:09Z08:00",
		"data": {
			"event": "tag_added",
			"repname": "repo1",
			"itemname": "item2",
			"tag": "tag3"
		}
	}

	{
		"type": "item_event",
		"receiver": "zhang3@example.com",
		"sender": "",
		"time": "2015-11-10T15:06:09Z08:00",
		"data": {
			"event": "item_deleted",
			"repname": "repo1",
			"itemname": "item2"
		}
	}
	
	event可能为tag_added, item_deleted

### 订购申请事件

	{
		"type": "subsapply_event",
		"receiver": "zhang3@example.com",
		"sender": "",
		"time": "2015-11-10T15:06:09Z08:00",
		"data": {
			"subscriptionid": 1234567,
			"sellername": "li4@example.com"
			"repname":"NBA",
			"itemname":"bear",
			"supply_style":"batch",
			"applytime":"2015-11-10T15:04:05Z08:00",
			"expiretime":"2016-11-17T15:04:05Z08:00",
			"phase":7,
			"plan":{
				"money":5,
				"units":3,
				"used":0,
				"limit":0,
				"subs":1,
				"expire":30
			}
		}
	}

	{
		"type": "subsapply_event",
		"receiver": "zhang3@example.com",
		"sender": "",
		"time": "2015-11-10T15:06:09Z08:00",
		"data": {
			"subscriptionid": 1234567,
			"sellername": "li4@example.com"
			"repname":"NBA",
			"itemname":"bear",
			"supply_style":"batch",
			"signtime":"2015-11-10T15:04:05Z08:00",
			"expiretime":"2016-11-17T15:04:05Z08:00",
			"phase":110,
			"plan":{
				"money":5,
				"units":3,
				"used":0,
				"limit":0,
				"subs":1,
				"expire":30
			}
		}
	}

	{
		"type": "subsapply_event",
		"receiver": "zhang3@example.com",
		"sender": "",
		"time": "2015-11-10T15:06:09Z08:00",
		"data": {
			"subscriptionid": 1234567,
			"sellername": "li4@example.com"
			"repname":"NBA",
			"itemname":"bear",
			"supply_style":"batch",
			"agreetime":"2015-11-10T15:04:05Z08:00",
			"phase":10,
			"plan":{
				"money":5,
				"units":3,
				"used":0,
				"limit":0,
				"subs":1,
				"expire":30
			}
		}
	}
	
	phase可能为：
		applying: 7, 
		agreed_but_insufficient_balance: 10
		agreed: 110
		
### 会员续费提醒

	{
		"type": "vip_remind",
		"receiver": "zhang3@example.com",
		"sender": "",
		"level": 50,
		"time": "2015-11-10T15:06:09Z08:00",
		"data": {
			"level": 4
			"invalid": 7
		}
	}
	
	level：用户会员级别
	invalid：会员还有几天到期

## topic: to_emails.json

### 发送一条email

	{
		"to": "zhang3@example.com",
		"subject": "你的订购申请通过了",
		"content": "你的订购申请通过了，请可以下载数据了。http://hub.dataos.io/mySubscribe.html 。此邮件是系统自动转发，请不要回复。",
		"ishtml": false
	}

## topic: to_subscriptions.json

### 增加tag

	{
		"type": "0x00020000",
		"repname": "repo001",
		"itemname": "item002",
		"tag": "tag003",
		"time": "2015-11-10T15:06:09Z08:00"
	}

### 删除tag

	{
		"type": "0x00020001",
		"repname": "repo001",
		"itemname": "item002",
		"tag": "tag003",
		"time": "2015-11-10T15:06:09Z08:00"
	}

### 删除dataitem

	{
		"type": "0x00020002",
		"repname": "repo001",
		"itemname": "item002",
		"time": "2015-11-10T15:06:09Z08:00"
	}

### 删除repository (此事件已经取消，repository下的所有dataitems将发送一个被删除消息)

	{
		"type": "0x00020003",
		"repname": "repo001",
		"time": "2015-11-10T15:06:09Z08:00"
	}

## topic: to_users.json

### 管理员群发消息

	{
		"type": "0x00030000",
		"sender": "admin@example.com",
		"level": 0,
		"data": {
			"content": "bla bla ..."
		}
	}

	message服务期望user服务的返回格式(to_notifications.json)：
	(加一个receiver, 改type为admin_message)
	
	{
		"type": "admin_message",
		"receiver": "zhang3@example.com",
		"sender": "admin@example.com",
		"level": 0,
		"data": {
			"content": "bla bla ..."
		}
	}

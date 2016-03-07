# API 列表
	

- [POST] /heartbeat 发送心跳信息
- [GET] /heartbeat/status/:user 获取user的daemon status。

----------

## 指令：POST /heartbeat 发送心跳信息

说明
	心跳信息由Daemon根据配置的心跳周期发送给Server，作用之一为新Daemon上线后向Server注册；二为汇报Daemon的健康情况，上报自己的entrypoint；三为上传daemon的日志。

输入参数说明：
	
	daemonid: DaemonID，从网页端获取的安装脚本中获取。用于识别用户。
   	entrypoint: Daemon提供的入口信息。
    	log: daemon要上传的日志。
	role: daemon的角色，0为数据下载方，1为数据提供方
	abnormaltags: daemon本地健康检查发现出错的tag

Example Request：

	POST /heartbeat HTTP/1.1 
	Accept: application/json 
	Content-Type: application/json 
	

    {
        "daemonid":"0aef69daefb06d0afbe6c",
        "entrypoint":[
            "http://211.10.23.23:35800"
        ],
        "log":[
        "2015/12/07 09:45:12 testlog11",
        "2015/12/07 09:45:12 testlog12",
        "2015/12/07 09:45:12 testlog13",
        "2015/12/07 09:45:12 testlog14",
        "2015/12/07 09:45:12 testlog15",
        "2015/12/07 09:45:12 testlog16"
        ],
        "role":1,
        "abnormaltags":[
        "repo1/item1/tag1",
        "repo2/item2/tag2",
        "repo3/item3/tag3"
        ]	
    }

返回数据说明：

    无

返回数据示例
        
	HTTP/1.1 200 OK
	Accept: application/json 
	Content-Type: application/json 


## 指令：GET /heartbeat/status/:user 获取user的daemon status。

说明
	返回user的datahub daemon在线状态：online，offline。

输入参数说明：
	
    无

Example Request：

	GET /heartbeat/status/testuser@asiainfo.com HTTP/1.1 
	Accept: application/json 
	Content-Type: application/json 
	

返回数据示例
        
	HTTP/1.1 200 OK
	Accept: application/json 
	Content-Type: application/json 

    {
        "status":"online",
    }

	

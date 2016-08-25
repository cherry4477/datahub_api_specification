## Errors

    ErrorUnauthorized        = 6001
	ErrorUnmarshal           = 6002
	ErrorAddModel            = 6003
	ErrorGetModel            = 6004 

# API 列表


- [POST] /custom/datahub datahub页面新增一个需求
- [GET]  /custom/datahub/requirement?name={name}&phone={phone}&email={email}&company={company}&content={content}

----------

##1 指令 ：POST /custom/datahub

说明：
      
      datahub页面数据定制新增一个需求，用户可以不用登陆就可以提交一个需求。
      
输入参数说明：

        name: 提交人姓名
        phone：提交人联系电话
        email：提交人电子邮箱
        company：公司名称
        requirementContent：需求内容
      
输入样例：

        POST /custom/datahub HTTP/1.1
	    Accept: application/json 
		Content-Type: application/json 
	    
	    {
            "name":"张三",
            "phone":"13838385438",
            "email": "zhangsan@163.com",
            "company":"asiainfo",
            "requirementContent": "test"
	    }
      
输出样例：

        HTTP/1.1 200 OK
        Accept: application/json 
        Content-Type: application/json
        {
            "code": 0,
            "msg": "OK.",
            "data": null
        } 
	     
	     
##2 指令 ：GET /custom/datahub/requirement datahub页面根据输入参数查询需求

说明：

        datahub页面根据输入参数查询需求，用户需要登陆，只能查询自己提交的需求

输入参数说明：

        name: (可选)提交人姓名
        phone: (可选)提交人电话
        email: (可选)提交人电子邮件
        company: (可选)公司名称
        content: (可选)需求内容
        
输入样例：

        GET /custom/datahub/requirement?name=王猛 HTTP/1.1
	    Accept: application/json 
		Content-Type: application/json
		
输出样例：

        HTTP/1.1 200 OK
        Content-Length: 1607
        Content-Type: application/json; charset=utf-8
        Server: beegoServer:1.6.1
        Date: Thu, 25 Aug 2016 09:25:27 GMT
        
        {
          "code": 0,
          "msg": "OK.",
          "data": [
            {
              "id": 1,
              "name": "王猛",
              "phone": "111",
              "email": "xmwillgo@163.com",
              "company": "asiainfo",
              "requirementContent": "test1",
              "createUser": "",
              "requirementName": "",
              "attribute": "",
              "resourcemap": "",
              "trade": "",
              "scope": "",
              "scene": "",
              "frequency": "",
              "deliver": "",
              "priority": "",
              "Create_time": "2016-08-25T11:32:06+08:00",
              "Update_time": "2016-08-25T11:32:06+08:00",
              "remark": "",
              "status": "A"
            },
            {
              "id": 2,
              "name": "王猛",
              "phone": "111",
              "email": "xmwillgo@163.com",
              "company": "asiainfo",
              "requirementContent": "test1",
              "createUser": "",
              "requirementName": "",
              "attribute": "",
              "resourcemap": "",
              "trade": "",
              "scope": "",
              "scene": "",
              "frequency": "",
              "deliver": "",
              "priority": "",
              "Create_time": "2016-08-25T11:34:36+08:00",
              "Update_time": "2016-08-25T11:34:36+08:00",
              "remark": "",
              "status": "A"
            }
          ]
        }
        
返回数据说明：

        name: 提交人姓名
        phone: 提交人电话
        email: 提交人电子邮件
        company: 公司名称
        requirementContent: 需求内容
        createUser: 创建人
        requirementName: 需求名称
        attribute: 需求属性(内部、外部)
        resourcemap: 是否来源于数据资源地图
        trade: 行业
        scope: 数据需求范围（地区，时段，覆盖等）
        scene: 数据应用场景
        frequency: 更新频次
        deliver: 交付方式
        priority: 需求优先级
        Create_time: 创建时间
        Update_time: 更新时间
        remark: 备注
        status: 状态


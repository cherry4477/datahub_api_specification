## Errors

    ErrorUnauthorized        = 6001
	ErrorUnmarshal           = 6002
	ErrorAddModel            = 6003
	ErrorGetModel            = 6004 
    ErrorGetModelById        = 6005
	ErrorUpdateModel         = 6006
	ErrorDeleteModel         = 6007
	ErrorAtoi                = 6008
    ErrorValidateParams      = 6009

# API 列表


- [POST]   /custom/datahub
- [GET]    /custom/datahub/requirement?page={page}&size={size}&name={name}&phone={phone}&email={email}&company=                         {company}&content={content}
- [POST]   /custom/operation
- [GET]    /custom/operation/requirement?name={name}&phone={phone}&email={email}&company={company}&content={content}
- [GET]    /custom/operation
- [PUT]    /custom/operation/:reqId
- [DELETE] /custom/operation/:reqId

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
        Authorization: Token XXXXXXXXXXXXXXXXXXXXXXX
	    
	    {
            "name":"张三",
            "phone":"13838385438",
            "email": "zhangsan@163.com",
            "company":"asiainfo",
            "requirementContent": "test",
            "remark": "test111"
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
        
返回数据说明：

        无
        
	     
##2 指令 ：GET /custom/datahub/requirement?page={page}&size={size}&name={name}&phone={phone}&email={email}&company={company}&content={content}

说明：

        datahub页面根据输入参数查询需求，用户需要登陆，只能查询自己提交的需求

输入参数说明：
        
        page: (可选)第几页，默认为1
        size: (可选)一页的大小，默认为30，最小为1，最大为100
        name: (可选)提交人姓名
        phone: (可选)提交人电话
        email: (可选)提交人电子邮件
        company: (可选)公司名称
        content: (可选)需求内容
        
输入样例：

        GET /custom/datahub/requirement?name=王猛 HTTP/1.1
	    Accept: application/json 
		Content-Type: application/json
        Authorization: Token XXXXXXXXXXXXXXXXXXXXXXXX
		
输出样例：

        HTTP/1.1 200 OK
        Content-Length: 998
        Content-Type: application/json; charset=utf-8
        Server: beegoServer:1.6.1
        Date: Fri, 11 Nov 2016 03:31:04 GMT

        {
          "code": 0,
          "msg": "OK.",
          "data": {
            "total": 3,
            "results": [
              {
                "Name": "wm",
                "Phone": "111",
                "Email": "xmwillgo@163.com",
                "Company": "xmwillgo@163.com",
                "Requirement_content": "test1",
                "Create_time": "2016-11-11T11:05:27+08:00",
                "Remark": "",
                "Status": "需求提交"
              },
              {
                "Name": "wm",
                "Phone": "111",
                "Email": "xmwillgo@163.com",
                "Company": "xmwillgo@163.com",
                "Requirement_content": "test1",
                "Create_time": "2016-11-11T11:06:16+08:00",
                "Remark": "",
                "Status": "需求提交"
              },
              {
                "Name": "wm",
                "Phone": "111",
                "Email": "xmwillgo@163.com",
                "Company": "xmwillgo@163.com",
                "Requirement_content": "test1",
                "Create_time": "2016-11-11T11:06:17+08:00",
                "Remark": "",
                "Status": "需求提交"
              }
            ]
          }
        }
        
返回数据说明：

        name: 提交人姓名
        phone: 提交人电话
        email: 提交人电子邮件
        company: 公司名称
        requirementContent: 需求内容
        Create_time: 创建时间
        remark: 备注
        status: 状态
        
    
##3 指令 ：POST /custom/operation

说明：
        
        运营页面新增一个需求，需要登陆
        
输入参数说明：

        name: 提交人姓名
        phone：提交人联系电话
        email：提交人电子邮箱
        company：公司名称
        requirementContent：需求内容
        requirementName: 需求名称
        attribute: 需求属性(内部、外部)，输入I表示内部，E表示外部
        resourcemap: 是否来源于数据资源地图，输入Y表示是，N表示否
        trade: 行业
        scope: 数据需求范围（地区，时段，覆盖等）
        scene: 数据应用场景
        frequency: 更新频次
        deliver: 交付方式
        priority: 需求优先级，高，中，低
        remark: 备注
        
输入样例：

        POST /custom/operation HTTP/1.1
	    Accept: application/json 
		Content-Type: application/json

        {
        	"name":"张三",
        	"phone":"1234567890",
        	"email":"xmwillgo@163.com",
        	"company":"asiainfo",
        	"requirementContent":"test1",
        	"requirementName":"test",
        	"attribute":"I",
        	"resourcemap":"Y",
        	"trade":"guangzhou",
        	"scope":"trasportation",
        	"scene":"yingyong",
        	"frequency":"高",
        	"deliver":"aaa",
        	"priority":"高",
        	"remark":"test"
        }
        
输出样例：
        
        HTTP/1.1 200 OK
        Content-Length: 47
        Content-Type: application/json; charset=utf-8
        Server: beegoServer:1.6.1
        Date: Tue, 06 Sep 2016 03:13:25 GMT
        
        {
          "code": 0,
          "msg": "OK.",
          "data": null
        }
        
返回数据说明：

        无
        
        
##4 指令 ：GET /custom/operation/requirement?name={name}&phone={phone}&email={email}&company={company}&content={content}

说明：

        运营页面根据输入的参数查询需求，用户需要登陆，可以查询所有的需求
        
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
		
输入样例：

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
        
        
##5 指令 ：GET /custom/operation

说明：

        获取所有的需求，用户需要登陆
        
输入参数说明：

        无
        
输入样例：

        GET /custom/operation HTTP/1.1
	    Accept: application/json 
		Content-Type: application/json
		
输出样例：

        HTTP/1.1 200 OK
        Content-Length: 118466
        Content-Type: application/json; charset=utf-8
        Server: beegoServer:1.6.1
        Date: Tue, 06 Sep 2016 03:48:47 GMT
        
        {
          "code": 0,
          "msg": "OK.",
          "data": [
            {
              "id": 1,
              "name": "张三",
              "phone": "13838385438",
              "email": "zhangsan@163.com",
              "company": "asiainfo",
              "requirementContent": "test",
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
              "Create_time": "2016-08-26T18:28:48+08:00",
              "Update_time": "2016-08-26T18:28:48+08:00",
              "remark": "",
              "status": "A"
            },
            {
              "id": 2,
              "name": "张三",
              "phone": "13838385438",
              "email": "zhangsan@163.com",
              "company": "asiainfo",
              "requirementContent": "test",
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
              "Create_time": "2016-08-26T18:55:38+08:00",
              "Update_time": "2016-08-26T18:55:38+08:00",
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
    
        
##6 指令 ：PUT /custom/operation/:reqId

说明：

        更新一个需求，用户需要登陆
        
输入参数说明：

        reqID：需要更新的需求的ID（必填）
        
输入样例：

        PUT /custom/operation/221 HTTP/1.1
	    Accept: application/json 
		Content-Type: application/json
		
		{
          "phone":"1111111111"
        }
        
输出样例：

        HTTP/1.1 200 OK
        Content-Length: 47
        Content-Type: application/json; charset=utf-8
        Server: beegoServer:1.6.1
        Date: Tue, 06 Sep 2016 06:16:18 GMT
        
        {
          "code": 0,
          "msg": "OK.",
          "data": null
        }

返回数据说明：

        无
        

##7 指令 ：DELETE /custom/operation/:reqId

说明：

        根据ID删除需求，删除只是把字段status置为N
        
输入参数说明：

        reqID：需要更新的需求的ID（必填）
        
输入样例：

        DELETE /custom/operation/221 HTTP/1.1
	    Accept: application/json 
		Content-Type: application/json
		
输出样例

        HTTP/1.1 200 OK
        Content-Length: 47
        Content-Type: application/json; charset=utf-8
        Server: beegoServer:1.6.1
        Date: Tue, 06 Sep 2016 06:27:24 GMT
        
        {
          "code": 0,
          "msg": "OK.",
          "data": null
        }
        
返回参数说明：

        无

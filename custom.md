# API 列表


- [POST] /custom/datahub datahub页面新增一个需求

----------

##1 指令 ：POST /custom/datahub datahub页面新增一个需求

说明：
      
      datahub页面数据定制新增一个需求，用户可以不用登陆就可以提交一个需求。
      
输入参数说明：

        name: 提交人姓名
        phone：提交人联系电话
        email：提交人电子邮箱
        company：公司名称
        requirementContent：需求内容
      
Example Request：

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
	    
返回数据说明：

      无
      
返回数据示例

    HTTP/1.1 200 OK
    Accept: application/json 
    Content-Type: application/json
    {
        "code": 0,
        "msg": "OK.",
        "data": null
    } 
	          

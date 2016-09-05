## Errors

    ErrorUnmarshal         = 6001
	ErrorMarshal           = 6002
	ErrorSendAsyncMessage  = 6003
	ErrorReadAll           = 6004 

# API 列表


- [GET] /alarm 
- [POST] /alarm 

----------

##1 指令 ：GET /alarm

说明：

        查询微信告警服务是否正常运行
        
输入参数说明：
        
        无
        
输入样例：
        
        GET /alarm HTTP/1.1
        Accept: application/json 
		Content-Type: application/json
		
输出样例：

        HTTP/1.1 200 OK
        Content-Type: application/json; charset=utf-8
        Date: Mon, 05 Sep 2016 09:29:22 GMT
        Content-Length: 50
        
        {"code":0,"msg":"Service is running.","data":null}
        
返回参数说明：
        
        无
        
        
##2 指令 ：POST /alarm

说明：
        
        发送一个微信告警
        
输入参数说明：

        sender：发送者
        content：告警内容
        
输入样例：

        POST /alarm HTTP/1.1
	    Accept: application/json 
		Content-Type: application/json 
	    
	    {
            "sender":"wangmeng",
            "content":"test"
	    }
	    
输出样例：
        
        HTTP/1.1 200 OK
        Content-Type: application/json; charset=utf-8
        Date: Mon, 05 Sep 2016 09:36:30 GMT
        Content-Length: 34
        
        {"code":0,"msg":"OK.","data":null}

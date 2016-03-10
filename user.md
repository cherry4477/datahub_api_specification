# API 列表
	

- [GET] /users/:loginname 查询用户

- [GET] /users/search/user 查询用户列表

- [POST] /users/:loginname 创建用户

- [PUT] /users/:loginname 修改用户

- [DELETE] /users/:loginname 删除用户

- [PUT] /users/:loginname/active 激活用户

- [PUT] /users/:loginname/pwd 修改密码

- [GET] /users/:loginname/pwd/validate 验证密码是否正确

- [PU]T /users/:loginname/forget/pwd 忘记密码发邮件

- [GET] /users/filtrate 查询不存在用户

- [GET] /quota/:loginname/repository 获取repo配额信息

- [POST] /quota/:loginname/repository 创建repo配额信息

- [PUT] /quota/:loginname/repository 修改repo配额

- [POST] /quota/:loginname/repository/use 修改repo的使用量

- [GET] /quota/:loginname/deposit 获取托管配额信息

- [POST] /quota/:loginname/deposit 新建托管配额

- [PUT] /quota/:loginname/deposit 修改用户的托管配额

- [GET] /quota/:loginname/pullnum 获取用户下载量配额信息

- [POST] /quota/:loginname/pullnum/ 创建用户下载量配额

- [PUT] /quota/:loginname/pullnum 修改用户下载量配额

- [POST] /quota/:loginname/pullnum/use 修改用户的已下载量

- [GET] /vip/:loginname 查询会员信息

- [GET] /vip/:loginname/cost 查看用户要升级的会员所需金额

- [PUT] /vip/:loginname 修改会员信息

- [POST] /massage/send 发送信息

- [POST] /massage/save 保存草稿

- [DELETE] /massage/draft 删除草稿

- [PUT] /massage/update 更新信息

- [GET] /massage/queryMassage 查询信息


	
----------

##指令：GET /users/:loginname 查询用户(81)
	说明
		【任意】 返回一个用户的详细情况，如果是自己，可以获得更详细的情况，如何是其他人，获得基本情况
	输入参数说明：
		无
	Example Request：
		GET /users/foo HTTP/1.1 
		Accept: application/json;charset=UTF-8

	返回数据说明：
		code:状态码
		msg:操作信息，用来记录失败信息
		data：数据结果

		comment：描述信息
		invalidTime:失效时间
		nickname：昵称 
		registTime:注册时间
		userId:用户id
		username:真实名称
		userStatus:用户状态（1：未激活，2：激活，3：失效，4：删除）
		usertype：用户类型(1：普通用户，2：管理员用户,3:认证会员,4：金卡会员，5钻石会员)
		
	返回数据示例
		{"data":{"comment":"abc","invalidTime":"2016-12-01","nickName":"foo","registTime":"2015-12-01","userId":1015,"userName":"FOO","userStatus":2,"userType":1},"code":0,"msg":"ok"}

##指令：GET /users/search/user 查询用户列表
	说明
		【管理员】 分页查询用户列表（查询条件可选）
	输入参数说明：
		page:当前页数
		size:每页数据量
		loginName:登录名，（模糊匹配）
		userName:真实名称，（模糊匹配）
		userType：用户类型（1：普通用户，2:认证会员,3：金卡会员，4钻石会员）
		userNameCompare：（1：等于，2：不等于，3：包含，4：不包含）
		loginNameCompare：（1：等于，2：不等于，3：包含，4：不包含）
		userTypeCompare：（1：等于，2：不等于，3：包含，4：不包含）
	Example Request：
		GET /users/search/user?page=1&size=20&loginName=asiainfo&userName=foo&userType=1&userNameCompare=3&loginNameCompare=3&userTypeCompare=1 HTTP/1.1 
		Accept: application/json;charset=UTF-8

	返回数据说明：
		code:状态码
		msg:操作信息，用来记录失败信息
		total：总记录数
		data：数据集合 
		comment：描述信息
		invalidTime:失效时间
		nickname：昵称 
		loginName：登录名
		registTime:注册时间
		userId:用户id
		username:真实名称
		userStatus:用户状态（1：未激活，2：激活，3：失效，4：删除）
		usertype：用户类型(1：普通用户，2：管理员用户,3:认证会员,4：金卡会员，5钻石会员)
	返回数据示例
		{"data":{"total":86,"results":[{"comment":"abc","invalidTime":"2016-12-01","nickName":"foo","registTime":"2015-12-01","userId":1025,"userName":"FOO","loginName":"guolq3@asiainfo.com","userStatus":2,"userType":1}]},"code":0,"msg":"ok"}

##指令：POST /users/:loginname 创建用户(82)
	说明：
		【任意】创建一个用户
	输入参数说明：
		passwd：MD5以后的密码
	Example Request：
		POST /users/foo?passwd=abc
	
	返回数据说明：
		code:状态码
		msg:操作信息，用来记录失败信息
	返回数据示例
		{"code":0,"msg":"ok"}

##指令：PUT /users/:loginname/active 激活用户(83)
	说明：
		【任意】激活用户
	输入参数说明：
		sid:秘钥
	Example Request：
		PUT /users/aaa@126.com/active HTTP/1.1 
		Content-Type: application/json;charset=UTF-8
 
		{"sid":"aaaaaa"}
	返回数据说明
		code:状态码
		msg:操作信息，用来记录失败信息
	返回数据示例
		{"code":0,"msg":"ok"}

##指令：PUT /users/:loginname/pwd 修改密码(84)
	说明：
		【自己或管理员】修改用户密码
		输入参数说明：
		oldpwd：修改前密码（md5）
		passwd：修改后密码（md5）
	Example Request：
		PUT /users/aaa@126.com/pwd HTTP/1.1 
		Content-Type: application/json;charset=UTF-8
		Authorization: token

		{"passwd":"aaaaaa","oldpwd":"1234"}
	返回数据说明
		code:状态码
		msg:操作信息，用来记录失败信息
	返回数据示例
		{"code":0,"msg":"ok"}

##指令：PUT /users/:loginname/pwd/reset 重置密码
	说明：
		【任意】重置密码
		输入参数说明：
		passwd：修改后密码（md5）
		sid:秘钥.验证标识（用于忘记密码后的重置密码）
	Example Request：
		PUT /users/aaa@126.com/pwd/reset HTTP/1.1 
		Content-Type: application/json;charset=UTF-8
 
		{"sid":"aaaaaa","passwd":"1234"}
	返回数据说明
		code:状态码
		msg:操作信息，用来记录失败信息
	返回数据示例
		{"code":0,"msg":"ok"}


##指令：PUT /users/:loginname 修改用户(85)
	【管理员角色】 说明 ：
		修改一个用户
	【管理员角色】输入参数说明：
		userstatus:用户状态（除了激活状态，其他状态需要有管理员权限）
		nickname：昵称
		username：真实名称
		comments：描述信息
		passwd：密码(MD5)

	【管理员角色】Example Request：
		PUT /users/foo HTTP/1.1 
		Content-Type: application/json;charset=UTF-8
		Authorization: token
 
		{
			"userstatus":3,
			"nickname":"foo",
			"username":"FOO",
			"comments":"测试用户",
			"passwd":".........."
		}

	【普通用户】 说明 ：
		修改一个用户
	【普通用户】输入参数说明：

		{
			nickname：昵称
			comments：描述信息
		}

	【普通用户】Example Request：
		PUT /users/foo HTTP/1.1 
		Content-Type: application/json;charset=UTF-8
		Authorization: token
 	
		{
			"nickname":"foo",
			"comments":"测试用户"
		}
	返回数据说明
		code:状态码
		msg:操作信息，用来记录失败信息
	返回数据示例
		{"code":0,"msg":"ok"}

##指令：DELETE /users/:loginname 删除用户(86)
	说明：
		【管理员角色】删除一个用户
		注：本操作不是真的删除一条数据，是将用户状态改成 注销状态（user_status:7）
	输入参数说明：
		无 
	Example Request：
		DELETE /users/foo HTTP/1.1 
		Content-Type: application/json;charset=UTF-8
		Authorization: token
                  
	返回数据说明
		code:状态码
		msg:操作信息，用来记录失败信息
	返回数据示例
		{"code":0,"msg":"ok"}
##指令：GET /users/:loginname/pwd/validate 验证密码是否正确(86.5)
	说明：
		【任意】验证密码是否正确
	输入参数说明：
		pwd:用户密码(md5加密过的)
	Example Request：
		GET /users/foo@asiainfo.com/pwd/validate?pwd=abc HTTP/1.1 
		Content-Type: application/json;charset=UTF-8
                  
	返回数据说明
		code:状态码 0：密码正确，8004：密码错误，1007：参数错误，1001：其他系统异常
		msg:操作信息，用来记录失败信息
	返回数据示例
		{"code":0,"msg":"ok"}
##指令：PUT /users/:loginname/forget/pwd 忘记密码发邮件
	说明：
		【任意】忘记密码发邮件
	输入参数说明：
		无
	Example Request：
		PUT /users/liuxy10/forget/pwd HTTP/1.1 
		Content-Type: application/json;charset=UTF-8
                  
	返回数据说明
		code:状态码 0：密码正确，1001：其他系统异常
		msg:操作信息，用来记录失败信息
	返回数据示例
		{"code":0,"msg":"ok"}
		
##指令：PUT /users/exist 查询不存在用户
	说明
		【管理员】 查询不存在用户
	输入参数说明：
		page:当前页数
		size:每页数据量
		loginNames:登录名集合
	Example Request：
		GET /users/exist HTTP/1.1 
		Accept: application/json;charset=UTF-8
		Content-Type: application/json;charset=UTF-8
		{
			"page":1
			"size":20
			"loginNames"：["guolq3@asiainfo.com"，"bac"]
		}
	返回数据说明：
		code:状态码
		msg:操作信息，用来记录失败信息
		total：总记录数
		data：数据集合 
		loginNames:不存在的用户
	返回数据示例  
		{"data":{"total":86,"results":[{"loginNames":["guolq3@asiainfo.com"，"bac"]}]},"code":0,"msg":"ok"}
		
##指令：PUT /users/:loginname/resend/active 重新发送激活邮件
	说明：
		【任意】忘记密码发邮件
	输入参数说明：
		无
	Example Request：
		PUT /users/liuxy10/resend/active HTTP/1.1 
		Content-Type: application/json;charset=UTF-8
                  
	返回数据说明
		code:状态码 0：密码正确，1001：其他系统异常
		msg:操作信息，用来记录失败信息
	返回数据示例
		{"code":0,"msg":"ok"}

##指令：GET /quota/:loginname/repository 获取用户的repo配额（87）
	说明：
		【自己或者管理员】获取用户的配额信息
	输入参数说明：
		无
	Example Request：
		GET /quota/foo/repository HTTP/1.1 
		Content-Type: application/json
		
	返回数据说明
		code:状态码
		msg:操作信息，用来记录失败信息
		quotaPublic:public的repo的配额
		usePublic：Public的repo的使用量
		quotaPrivate:Private的repo的配额
		usePrivate：Private的repo的使用量
	返回数据示例
		{"code":0,"msg":"ok",
			data:{
				"quotaPublic":"10","usePublic":"5",
				"quotaPrivate":"15","usePrivate":"10"
				}
		}
##指令：POST /quota/:loginname/repository 增加用户的repo配额（88）
	说明：
		【管理员角色】添加用户的repo配额信息
	输入参数说明：
		private:私有repo配额数量
		public:共有repo配额数量

	Example Request：
		POST /quota/foo/repository HTTP/1.1 
		Content-Type: application/json;charset=UTF-8
		Authorization: token
		{
			"private":20,
			"public":50
		}	
	返回数据说明
		code:状态码
		msg:操作信息，用来记录失败信息
	返回数据示例
		{"code":0,"msg":"ok"}
##指令：PUT /quota/:loginname/repository 修改用户的repo配额（89）
	说明：
		【管理员角色】修改用户的repo的配额
	输入参数说明：
		private:私有repo配额数量
		public:共有repo配额数量
		注：输入参数可单独使用
	Example Request：
		PUT /quota/foo/repository HTTP/1.1 
		Content-Type: application/json;charset=UTF-8
		Authorization: token
		{
			"private":30,
			"public":60
		}
	返回数据说明
		code:状态码
		msg:操作信息，用来记录失败信息
	返回数据示例
		{"code":0,"msg":"ok"}
##指令：POST /quota/:loginname/repository/use 修改用户的repo使用量（8a）
	说明：
		【管理员】修改用户的repo的使用量
		注：只允许本人或者管理员修改
	输入参数说明：
		private:私有repo使用数量
		public:共有repo使用数量）
		注：输入参数可单独使用
	Example Request：
		POST /quota/foo/repository/use HTTP/1.1 
		Content-Type: application/json;charset=UTF-8
		Authorization: token
		{
			"private":1,
			"public":1
		}
	返回数据说明
		code:状态码
		msg:操作信息，用来记录失败信息
	返回数据示例
		{"code":0,"msg":"ok"}


##指令：GET /quota/:loginname/deposit 获取用户的托管信息（8b）
	说明：
		【自己或者管理员】获取用户的托管的配额信息
	输入参数说明：
		无
	Example Request：
		GET /quota/foo/deposit HTTP/1.1 
		Content-Type: application/json;charset=UTF-8
		Authorization: token

	返回数据说明
		code:状态码
		msg:操作信息，用来记录失败信息
		quota:托管的配额
		use：托管使用量
	返回数据示例
		{"code":0,"msg":"ok",
			data:{
				"quota":"10M","use":"5M"
				}
		}
##指令：POST /quota/:loginname/deposit 增加用户的托管配额（8c）
	说明：
		【管理员】添加用户的托管配额信息 
	输入参数说明：
		quota:配额空间
		unit:单位

	Example Request：
		POST /quota/foo/deposit HTTP/1.1 
		Content-Type: application/json;charset=UTF-8
		Authorization: token

		{
			"quota":200,
			"unit":"M"
		}	
	返回数据说明
		code:状态码
		msg:操作信息，用来记录失败信息
	返回数据示例
		{"code":0,"msg":"ok"}
##指令：PUT /quota/:loginname/deposit 修改用户的托管配额（8d）
	说明：
		【管理员】修改用户的托管的配额 
	输入参数说明：
		quota:托管配额空间大小
	Example Request：
		PUT /quota/foo/deposit HTTP/1.1 
		Content-Type: application/json;charset=UTF-8
		Authorization: token
		{
			"quota":300
		}
	返回数据说明
		code:状态码
		msg:操作信息，用来记录失败信息
	返回数据示例
		{"code":0,"msg":"ok"}


##指令：GET /quota/:loginname/pullnum 获取用户的下载量信息（8f）
	说明：
		【自己或者管理员】获取用户的下载量信息
	输入参数说明：
		无
	Example Request：
		GET /quota/foo/pullnum HTTP/1.1 
		Content-Type: application/json;charset=UTF-8
		Authorization: token
	返回数据说明
		code:状态码
		msg:操作信息，用来记录失败信息
		quota:pull量的配额
		use：实际pull量
	返回数据示例
		{"code":0,"msg":"ok",
			data:{
				"quota":"10000","use":"100"
				}
		}
##指令：POST /quota/:loginname/pullnum 增加用户的下载量配额信息（8g）
	说明：
		【管理员】添加用户的托管配额信息 
	输入参数说明：
		quota:配额空间

	Example Request：
		POST /quota/foo/pullnum HTTP/1.1 
		Content-Type:application/json;charset=UTF-8
		Authorization: token

		{
			"quota":20000
		}	
	返回数据说明
		code:状态码
		msg:操作信息，用来记录失败信息
	返回数据示例
		{"code":0,"msg":"ok"}
##指令：POST /quota/:loginname/pullnum/use 修改用户的已下载量（8h）

	说明：
 	   【管理员】修改用户的已下载量
	输入参数说明：
   	 	use:下载数量
	Example Request：
    	POST /quota/foo/pullnum/use HTTP/1.1 
		Content-Type:application/json;charset=UTF-8
		Authorization: token
		{
       	 "use":10
		}
	返回数据说明
		code:状态码，(0：成功；7000:未知错误；7005：未登录；7006：权限不够)
		msg:操作信息，用来记录失败信息
	返回数据示例
		{"code":0,"msg":"ok"}

##指令：PUT /quota/:loginname/pullnum 修改用户的下载量配额（8i）
	说明：
		【管理员】修改用户的下载量的配额 
	输入参数说明：
		quota:下载量配额
	Example Request：
		PUT /quota/foo/pullnum HTTP/1.1 
		Content-Type: application/json;charset=UTF-8
		Authorization: token
		{
			"quota":300000
		}
	返回数据说明
		code:状态码
		msg:操作信息，用来记录失败信息
	返回数据示例
		{"code":0,"msg":"ok"}
##指令：GET /vip/:loginname 查询用户会员信息（8j）
	说明：
		【任意】查看用户会员的相关信息
	输入参数说明：
		无
	Example Request：
		GET /vip/foo HTTP/1.1 
		Accept: application/json;charset=UTF-8
		Authorization: token

	返回数据说明：
		userType:会员级别
		repoPub:共有repo资源量
		repoPri:私有repo资源量
		pullNum:免费数据Pull量
		payWay:pull付费方式 (1:预付费，2后付费)
		deposit：托管配额
		fee:年费
	返回数据示例：

		{"data":	{
			"deposit":"0M",
			"fee":0,
			"payWay":0,
			"pullNum":100,
			"repoPri":10,
			"repoPub":20,
			"userType":0
			},
		"code":0,"msg":"ok"
		}
#指令：GET /vip/:loginname/cost 查看用户要升级的会员所需金额(8k)
	说明：
		【自己活管理员】查看用户要升级的会员所需金额
	输入参数说明：
		type：会员级别
	Example Request：
		GET /vip/foo/cost?type=4 HTTP/1.1 
		Accept: application/json;charset=UTF-8
		Authorization: token
	返回数据说明：
		cost:金额
	返回数据示例：

		{"data":	{
			"cost":"1400"
			},
		"code":0,"msg":"ok"
		}

#指令：PUT /vip/:loginname 修改会员信息(8l)
	说明：
		【管理员】修改用户的会员信息,同时修改用户配额、以及扣取年费（管理员除外）
               1 会员续费 userType与以前不变，扣取对应的年费（整年）
				 1）以前存在有效期，并且没有到期，就按照有效期时间+会员有效时间 生成新的失效时间；
				 2）以前不存在有效期或者已到期，按照当前申请时间+会员有效时间 生成新的失效时间；
			   2 升级会员 userType与以前变化，修改配额；会员到期时间，从当前开始到下一年
				 1）年费扣取时，如果原会员未到期，算一下剩余的年费，收取升级的年费时，扣除前一次的剩余年费，会员到期时间，从当前开始到下一年
	           3 降级会员：未到期时不能降级，到期后可降级，修改配额，扣取年费，修改失效时间

	输入参数说明：
		userType：会员级别(1：普通用户，2：管理员用户,3:认证会员,4：金卡会员，5钻石会员)
		cost:费用
	Example Request：
		PUT /vip/foo HTTP/1.1 
		Accept: application/json;charset=UTF-8
		Authorization: token
		{
			"userType":3,
			"cost":1400
		}
	返回数据说明：
		code:状态码
		msg:操作信息，用来记录失败信息
	返回数据示例：
		{"code":0,"msg":"ok"}
		
##指令：POST /massage/send 发送信息
    说明：
		【管理员角色】发送信息
	输入参数说明：
        	users：收件人
		title：信息的标题
        	content：信息的内容
		type:信息的类型（1：消息，2：邮件，3:全选）
		sendMode:发送方式（1：立即发送，2：定时发送）
		time：定时时间（立即发送为空）
	Example Request：
		POST /massage/send
		Content-Type: application/json;charset=UTF-8
		{
			"users"：["guolq3@asiainfo.com"，"bac"]
			"title"：信息的标题
        		"content"：信息的内容
        		"type":1
			"sendMode":2
			"time"：2016-3-10
		}	
	返回数据说明：
		code:状态码
		msg:操作信息，用来记录失败信息
	返回数据示例
		{"code":0,"msg":"ok"}

##指令：POST /massage/save 保存草稿
    说明：
		【管理员角色】保存草稿
	输入参数说明：
        users：收件人
		title：信息的标题
        	content：信息的内容
	 	type:信息的类型（1：消息，2：邮件）
		sendMode:是否定时发送（1：立即发送，2：定时发送）
		time：定时时间（立即发送为空）
	Example Request：
		POST /massage/guolq3@asiainfo.com/save
		Content-Type: application/json;charset=UTF-8
		{
			"users"：["guolq3@asiainfo.com"，"bac"]
			"title"：信息的标题
        		"content"：信息的内容
        		"type":2
			"timing":1
			"time"：2016-3-10 13:22:11
		}	
	返回数据说明：
		code:状态码
		msg:操作信息，用来记录失败信息
	返回数据示例
		{"code":0,"msg":"ok"}

##指令：DELETE /massage/draft 删除草稿
	说明：
		【管理员角色】删除草稿
	输入参数说明：
		massageId：信息id
	Example Request：
		DELETE /massage/draft？massageId=1 HTTP/1.1 
		Content-Type: application/json;charset=UTF-8     
	返回数据说明
		code:状态码
		msg:操作信息，用来记录失败信息
	返回数据示例
		{"code":0,"msg":"ok"}

##指令：PUT /massage/update 更新信息
    说明：
		【管理员角色】更新信息
	输入参数说明：
		massageId：信息Id
        	users：收件人
		title：信息的标题
        	content：信息的内容
        	type:信息的类型（1：消息，2：邮件）
		sendMode:发送方式（1：立即发送，2：定时发送）
		time：定时时间
	Example Request：
		PUT /massage/update
		Content-Type: application/json;charset=UTF-8
		{
			"massageId":1
			"users"：["guolq3@asiainfo.com"，"bac"]
			"title"：信息的标题
        		"content"：信息的内容
        		"type":2
			"sendMode":2
			"time"：2016-3-10 13:20:10
		}	
	返回数据说明：
		code:状态码
		msg:操作信息，用来记录失败信息
	返回数据示例
		{"code":0,"msg":"ok"}

##指令：GET /users/queryMassage 查询信息
	说明
		【管理员】 查询信息
	输入参数说明：
		page:当前页数
		size:每页数据量
		massageState:信息状态（1：已发送 2：未发送）
	Example Request：
		GET /users/queryMassage?page=1&size=20&massageState=1HTTP/1.1 
		Accept: application/json;charset=UTF-8

	返回数据说明：
		code:状态码
		msg:操作信息，用来记录失败信息
		total：总记录数
		data：数据集合 
		title：标题
		user:用户名
		sendTime:发送时间
		createTime：创建时间
		massagetype：信息类型(1：消息，2：邮件)
	返回数据示例  
		{"data":{"total":86,"results":[{"title":"abc","user":"张三","sendTime":"2015-12-01 13：22：22","createtime":"2015-12-01 13：22：22","userType":1}]},"code":0,"msg":"ok"}

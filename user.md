# API 列表
	

- [GET] /users/:loginname 查询用户

- [GET] /users/search/user 查询用户列表

- [POST] /users/:loginname 创建用户

- [PUT] /users/:loginname 修改用户

- [DELETE] /users/:loginname 删除用户

- [PUT] /users/:loginname/active 激活用户

- [PUT] /users/:loginname/pwd 修改密码

- [GET] /users/:loginname/pwd/validate 验证密码是否正确

- [PUT] /users/:loginname/forget/pwd 忘记密码发邮件

- [GET] /users/:loginname/validateLink  验证连接是否有效

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

- [POST] /broadcasts 创建群发任务

- [DELETE] /broadcasts/:broadcastId 删除群发任务

- [PUT] /broadcasts/:broadcastId 更新群发任务

- [GET] /broadcasts?Id=:broadcastId 查询群发任务

- [PUT] /certification/person/:loginname 添加/修改个人认证信息

- [PUT] /certification/company/:loginname 增加/修改企业认证信息

- [GET] /certification/person/:loginname 查询个人认证信息

- [GET] /certification/company/:loginname 查询企业认证信息

- [PUT] /certification/inspection/:loginname 审核

- [GET] /certification/inspections 查询审核信息列表

- [POST] /certification/upload 上传图片

- [GET] /certification/download 查询图片
	
----------

##指令：GET /users/:loginname 查询用户(81)
<p style="color:red">需要修改的地方：sregion 合并到loginname中， so 输入信息中取消sregion，返回信息不变。</p>
需要修改的地方：sregion 合并到loginname中， so 输入信息中取消sregion，返回信息不变。
	说明
		【任意】 返回一个用户的详细情况，如果是自己，可以获得更详细的情况，如果是其他人，获得基本情况
		loginname可以传入昵称
	输入参数说明：
		sregion:用户登陆来源
	Example Request：
		GET /users/foo?sregion=GZ HTTP/1.1 
		Accept: application/json;charset=UTF-8

	返回数据说明：
		code:状态码
		msg:操作信息，用来记录失败信息
		data：数据结果

		comment：描述信息
		invalidTime:失效时间
		loginname:登录名
		nickname：昵称 
		registTime:注册时间
		userId:用户id
		username:真实名称
		userStatus:用户状态（1：未激活，2：激活，3：认证，4：等待审核，5：审核未通过，7：账号销毁）
		usertype：用户类型(1：普通用户，2：管理员用户,3:认证会员,4：金卡会员，5钻石会员)
		tel:电话号码
		type：种类（1：个人，2：企业）
		headPic：用户头像id
		homepage:个人主页
		industry：所属行业
		person:业务接口人
		massage:审核不通过原因
		sregion:用户登录来源
	返回数据示例
		{"data":{"comment":"abc",
				"invalidTime":"2016-12-01",
				"loginname":"foo",
				"nickName":"foo",
				"registTime":"2015-12-01",
				"userId":1015,
				"userName":"FOO",
				"userStatus":2,
				"userType":1,
				"tel":"8008208820",
				"type":"1",
				"headPic":"xxxx",
				"homepage":"https://www.baidu.com/",
				"person":"郭立强",
				"industry":"地产",
				"massage":"bala bala"
				"sregion":"GZ"},"code":0,"msg":"ok"}


##指令：GET /users/search/user 查询用户列表
<p style="color:red">需要修改的地方：增加查询各个sregion的输入参数。需要区分管理员角色，比如gz的管理员，仅能查询gz的用户列表。</p>
	说明
		【管理员】 分页查询用户列表（查询条件可选）
	输入参数说明：
		page:当前页数
		size:每页数据量
		loginName:登录名，（模糊匹配）
		userName:真实名称，（模糊匹配）
		nickName:昵称，（模糊匹配）
		sregion: 用户登录来源
	Example Request：
		GET /users/search/user?page=1&size=20&loginName=a&userName=b&nickName=c &sregion=dHTTP/1.1 
		Accept: application/json;charset=UTF-8Th
		Authorization: token

	返回数据说明：
		code:状态码
		msg:操作信息，用来记录失败信息
		total：总记录数
		data：数据集合 
		comment：描述信息
		invalidTime:失效时间
		loginname:登录名
		nickname:昵称 
		registTime:注册时间
		userId:用户id
		username:真实名称
		userStatus:用户状态（1：未激活，2：激活，3：认证，4：等待审核，5：审核未通过，7：账号销毁）
		usertype：用户类型(1：普通用户，2：管理员用户,3:认证会员,4：金卡会员，5钻石会员)
		tel:电话号码
		type：种类（1：个人，2：企业）
		headPic：用户头像id
		homepage:个人主页
		industry：所属行业
		person:业务接口人
		massage:审核不通过原因
		sregion: 用户登录来源
	返回数据示例
		{"data":{"total":86,

				"results":[{"comment":"abc",
							"invalidTime":"2016-12-01",
							"loginname":"foo",
							"nickName":"foo",
							"registTime":"2015-12-01",
							"userId":1025,
							"userName":"FOO",
							"userStatus":2,
							"userType":1,
							"tel":"8008208820",
							"type":"1",
							"headPic":"xxxx",
							"homepage":"https://www.baidu.com/",
							"person":"郭立强",
							"industry":"地产",
							"massage":"bala bala",
							"sregion":"GZ"}]},"code":0,"msg":"ok"}

##指令：POST /users/:loginname 创建用户(82)
<p style="color:red">需要修改的地方：loginname的前两位字母为region值，由前端一起写入。前端生成passwd的方式为“取消loginname的前两个字母”md5写入。</p>
	说明：
		【非管理员】创建一个用户
		【管理员】创建一个激活用户
	输入参数说明：
		passwd：MD5以后的密码
		sregion: 用户登录来源
	Example Request：
		POST /users/foo@163.com?passwd=x1abcy2z3xdfpqghqpowifX11&sregion=datahub
	
	返回数据说明：
		code:状态码
		msg:操作信息，用来记录失败信息
	返回数据示例
		{"code":0,"msg":"ok"}

##指令：PUT /users/:loginname/active 激活用户(83)
<p style="color:red">需要修改的地方：loginname前两位字母为sregion值。由前端一起写入。</p>
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
<p style="color:red">需要修改的地方：loginname前两位字母为sregion值。新密码由loginname去掉前面2位字母后生成。需要区分管理员角色，gz管理员仅能修改gz用户的密码。</p>
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
<p style="color:red">需要修改的地方：loginname前两位字母为sregion值。修改后密码由loginname去掉前面2位字母后生成。</p>
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
<p style="color:red">需要修改的地方：loginname前两位字母为sregion值。新密码由loginname去掉前面2位字母后生成。需要区分管理员橘色，如gz管理员仅能修改gz用户的信息。</p>
	【管理员角色】 说明 ：
		修改一个用户
	【管理员角色】输入参数说明：
		userstatus:用户状态（除了激活状态，其他状态需要有管理员权限）
		nickname：昵称
		username：真实名称
		comments：描述信息
		passwd：密码(MD5)

		tel:电话号码
		headPic：用户头像id
		homepage:个人主页
		industry：所属行业
		person:业务接口人
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
        	"tel":"8008208820",
        	"homepage":"https://www.baidu.com/",
        	"headPic":"xxx",
        	"person":"郭立强",
			"industry":"地产"
		}

	【普通用户】 说明 ：
		修改一个用户
	【普通用户】输入参数说明：

		{
			nickname：昵称
			comments：描述信息
			tel:电话号码
			headPic：用户头像id
			homepage:个人主页
			industry：所属行业
			person:业务接口人
		}

	【普通用户】Example Request：
		PUT /users/foo HTTP/1.1 
		Content-Type: application/json;charset=UTF-8
		Authorization: token
 	
		{
			"nickname":"foo",
			"comments":"测试用户"
			"tel":"8008208820",
        	"homepage":"https://www.baidu.com/",
        	"headPic":"xxxx",
        	"person":"郭立强",
			"industry":"地产"
		}
	返回数据说明
		code:状态码
		msg:操作信息，用来记录失败信息
	返回数据示例
		{"code":0,"msg":"ok"}

##指令：DELETE /users/:loginname 删除用户(86)
<p style="color:red">需要修改的地方：loginname前两位字母为sregion值。需要区分管理员角色，如gz管理员仅能修改gz的用户。</p>
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
<p style="color:red">需要修改的地方：loginname前两位字母为sregion值。</p>
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
<p style="color:red">需要修改的地方：loginname前两位字母为sregion值。</p>
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
		

##指令：PUT /users/:loginname/resend/active 重新发送激活邮件
<p style="color:red">需要修改的地方：loginname前两位字母为sregion值。</p>
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

##指令：GET /users/:loginname/validateLink 查询连接是否有效(81)
<p style="color:red">需要修改的地方：loginname前两位字母为sregion值。</p>
	说明
		【任意】 返回测试连接是否还有效
	输入参数说明：
		无
	Example Request：
		GET /users/foo/validateLink?sid=abc HTTP/1.1 
		Accept: application/json;charset=UTF-8

	返回数据说明：
		code:状态码
		msg:操作信息，用来记录失败信息
		data：数据结果
		result:(1：有效，2：无效)
		
	返回数据示例
		{"data":{"result":1},"code":0,"msg":"ok"}


##指令：GET /quota/:loginname/repository 获取用户的repo配额（87）
<p style="color:red">需要修改的地方：loginname前两位字母为sregion值。需要区分管理员角色，如gz管理员仅能修改gz用户的配额。dh管理员仅能修改dh用户的配额。</p>
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
<p style="color:red">需要修改的地方：loginname前两位字母为sregion值。需要区分管理员角色，如gz管理员仅能修改gz用户的配额。dh管理员仅能修改dh用户的配额。</p>
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
<p style="color:red">需要修改的地方：loginname前两位字母为sregion值。需要区分管理员角色，如gz管理员仅能修改gz用户的配额。dh管理员仅能修改dh用户的配额。</p>
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
<p style="color:red">需要修改的地方：loginname前两位字母为sregion值。需要区分管理员角色，如gz管理员仅能修改gz用户的使用量。dh管理员仅能修改dh用户使用量。</p>
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
<p style="color:red">需要修改的地方：loginname前两位字母为sregion值。需要区分管理员角色，如gz管理员仅能修改gz用户的信息。dh管理员仅能修改dh用户信息。</p>
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
<p style="color:red">需要修改的地方：loginname前两位字母为sregion值。需要区分管理员角色，如gz管理员仅能修改gz用户的配额。dh管理员仅能修改dh用户配额。</p>
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
<p style="color:red">需要修改的地方：loginname前两位字母为sregion值。需要区分管理员角色，如gz管理员仅能修改gz用户的配额。dh管理员仅能修改dh用户配额。</p>
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
<p style="color:red">需要修改的地方：loginname前两位字母为sregion值。</p>
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
<p style="color:red">需要修改的地方：loginname前两位字母为sregion值。需要区分管理员角色，如gz管理员仅能修改gz用户的配额信息。dh管理员仅能修改dh用户的配额信息。</p>
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
<p style="color:red">需要修改的地方：loginname前两位字母为sregion值。需要区分管理员角色，如gz管理员仅能修改gz用户的下载量。dh管理员仅能修改dh用户的下载量。</p>

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
<p style="color:red">需要修改的地方：loginname前两位字母为sregion值。需要区分管理员角色，如gz管理员仅能修改gz用户的下载量。dh管理员仅能修改dh用户的下载量。</p>
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
<p style="color:red">需要修改的地方：loginname前两位字母为sregion值。需要区分来源返回，如gz来源的仅能查看广州用户会员信息。</p>
	说明：
		【任意】查看用户会员的相关信息
	输入参数说明：
		region: 用户登录来源
	Example Request：
		GET /vip/foo&sregion=a HTTP/1.1 
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
		sregion: 用户登录来源
	返回数据示例：

		{"data":	{
			"deposit":"0M",
			"fee":0,
			"payWay":0,
			"pullNum":100,
			"repoPri":10,
			"repoPub":20,
			"userType":0,
			"sregion":"GZ"
			},
		"code":0,"msg":"ok"
		}
##指令：GET /vip/:loginname/cost 查看用户要升级的会员所需金额(8k)
<p style="color:red">需要修改的地方：loginname前两位字母为sregion值。需要区分管理员角色，如gz管理员仅能查看gz用户的信息。dh管理员仅能查看dh用户信息。</p>
	说明：
		【自己或管理员】查看用户要升级的会员所需金额
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

##指令：PUT /vip/:loginname 修改会员信息(8l)
<p style="color:red">需要修改的地方：loginname前两位字母为sregion值。区分管理员角色，如gz管理员仅能修改gz会员信息。</p>
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

##指令：POST /broadcasts 创建群发任务
<p style="color:red">需要修改的地方：user 无sregion值。创建群发任务时需要前端带入sregion值。</p>
    说明：
		【管理员角色】创建群发任务
	输入参数说明：
        users：收件人列表
		title：信息的标题
        content：信息的内容
        massagetype:信息的类型(1：消息，2：邮件，3：全选)
		sendMode:发送方式（1：立即发送，2：定时发送）
		time：定时时间
	Example Request：
		POST /broadcasts
		Content-Type: application/json;charset=UTF-8
		{
			"users":["guolq3@asiainfo.com","771435128@qq.com"],
			"title":"信息的标题",
        	"content":"信息的内容",
        	"massageType":1,
			"sendMode":2,
			"time":"2016-03-11 13:22"
		}	
	返回数据说明：
		code:状态码
		msg:操作信息，用来记录失败信息
		data:broadcastId
	返回数据示例
		{"code":0,"msg":"ok",data:{"broadcastId":"123456"}}
		
##指令：DELETE /broadcasts/:broadcastId 删除群发任务  
<p style="color:red">需要修改的地方：区分管理员角色，如gz的管理员仅能删除gz的任务。</p>
	说明：
		【管理员角色】删除群发任务
	输入参数说明：
		无
	Example Request：
		DELETE /broadcasts/1 HTTP/1.1 
		Content-Type: application/json;charset=UTF-8     
	返回数据说明
		code:状态码
		msg:操作信息，用来记录失败信息
	返回数据示例
		正确 {"code":0,"msg":"ok"}
		错误 {"code":8013,"msg":"This broadcasts can not be deleted"}

##指令：PUT /broadcasts/:broadcastId 更新群发任务
<p style="color:red">需要修改的地方：区分管理员角色，如gz的管理员仅能更新gz的任务。</p>
    说明：
		【管理员角色】更新群发任务
	输入参数说明：
        users：收件人列表
		title：信息的标题
        content：信息的内容
        massagetype:信息的类型（1：消息，2：邮件，3：全选）
		sendMode:发送方式（1：立即发送，2：定时发送）
		time：定时时间
	Example Request：
		PUT /broadcasts/1
		Content-Type: application/json;charset=UTF-8
		{
			"users":["guolq3@asiainfo.com","771435128@qq.com"],
			"title":"信息的标题",
        	"content":"信息的内容",
        	"massagetype":1,
			"sendMode":2,
			"time":"2016-04-20 22:22"
		}	
	返回数据说明：
		code:状态码
		msg:操作信息，用来记录失败信息
	返回数据示例
		正确 {"code":0,"msg":"ok"}
		错误 {"code":8014,"msg":"This broadcasts can not be update"}
		
##指令：GET /broadcasts?Id=:broadcastId 查询群发任务
<p style="color:red">需要修改的地方：区分管理员角色，如gz的管理员仅能查询gz的任务。</p>
    说明：
		【管理员】 查询群发任务
	输入参数说明：
		broadcastId （可选）
		complete (可选  1:已经发送 2：所有未发）
		page:当前页数
    	size:每页数据量
	Example Request：
		GET /broadcasts?page=1&size=20&Id=1&complete=1 HTTP/1.1 
        Accept: application/json;charset=UTF-8
	返回数据说明：
		code:状态码
    	msg:操作信息，用来记录失败信息
    	data:{
    	  total:总记录数
    	  results:数据集合  {
    	    broadcastId 群发任务id
    	    title:标题
		    content：信息的内容
    	    users:用户群
    	    sendTime:发送时间
    	    massagetype：信息类型(1：消息，2：邮件，3：全选)
		    sendMode：发送方式（1：立即发送，2定时发送）
		    sendSuccess:发送成功的用户群(默认值为空)
		    sendFail:发送失败的用户群(默认值为空)
		    time：定时时间
		  }
		 }
	返回数据示例
		{"data":{"total":86,
				"results":[{"broadcastId":"1",
							"title":"abc",
							"content":"cde",
							"users":["guolq3@asiainfo.com","771435128@qq.com"],
							"sendTime":"2015-12-01 13:22",
							"massageType":1,
							"sendMode":1,
							"sendSuccess":"[]",
							"sendFail":"[]",
							"time":"2015-12-01 13:22"}]},
				"code":0,"msg":"ok"}


##指令：PUT /certification/person/:loginname 添加/修改个人认证信息
<p style="color:red">需要修改的地方：loginname强两位字母为region值。管理员区分角色，如gz管理员仅能修改gz用户的认证信息。</p>
    说明：
		【自己或管理员】添加/修改个人基本信息
	输入参数说明：
        name：姓名
		tel：电话号码
        idNum：身份证号
		idPicUp:身份证正面(图片的id)
		idPicDown:身份证正面(图片的id)
		accountName：账户名称
		bankNum:银行账号
		bank:银行名称
		bankName:开户支行名称
	Example Request：
		PUT /certification/person/:loginname
		Content-Type: application/json;charset=UTF-8
		{
			"name":"郭立强",
			"tel":"8008208820",
        	"idNum":"110109198711222014",
			"idPicUp":"xxx",
			"idPicDown":"xxx",
			"accountName":"郭立强",
			"bankNum":"201324832186131156",
			"bank":"xx银行",
			"bankName":"xx路支行"
		}	
	返回数据说明：
		code:状态码
		msg:操作信息，用来记录失败信息
	返回数据示例
		正确 {"code":0,"msg":"ok"}
		错误 {"code":1007,"msg":"invalid parameters"}
		错误 {"code":8016,"msg":"The user has passed authentication"}

##指令：PUT /certification/company/:loginname  添加/修改企业认证信息
<p style="color:red">需要修改的地方：loginname强两位字母为region值。管理员区分角色，如gz管理员仅能修改gz用户的认证信息。</p>
    说明：
		【自己或管理员】添加/修改企业基本信息
	输入参数说明：
        name:企业名称
		address:公司地址
		kbisNum:营业执照编号
		kbisPic:营业执照扫描件(图片id)
		org：组织结构代码
		orgPiv:组织结构代码扫描件(图片id)
		legalPerson：法人姓名
		legalPersonPicUp：法人身份证正面(图片id)
		legalPersonPicDown：法人身份证反面(图片id)
		legalPersonAddress：法人归属地
		legalPersonNum：法人身份证号
		person：联系人姓名
		personPicUp：联系人身份证正面(图片id)
		personPicDown：联系人身份证反面(图片id)
		personTel：联系人电话
		personEmail：联系人电子邮箱
		personNum:联系人身份证号
		verityPic：授权证书扫描件(图片id)
		accountName：账户名称
		bankNum:银行账号
		bank:银行名称
		bankName:开户支行名称
	Example Request：
		PUT /certification/company/:loginname
		Content-Type: application/json;charset=UTF-8
		{
			"name":"强大大有限公司",
			"address":"北京xxxx",
        	"kbisNum":"11010919871",
			"kbisPic":"xxx",
        	"org":"我是组织结构",
			"legalPerson":"张三",
			"legalPersonPicUp":"zxx",
			"legalPersonPicDown":"zxx",
			"legalPersonAddress":"某个村子里",
			"legalPersonNum":"230123285133458132",
			"person":"郭立强",
			"personPicUp":"xxx",
			"personPicDown":"xxx",
			"personTel":"80082052513",
			"personEmail":"abc@abc.com",
			"personNum":"230318132183218",
			"verityPic":"xxx",
			"accountName":"强大大有限公司"
			"bankNum":"201324832186131156",
			"bank":"xx银行",
			"bankName":"xx路支行"
		}	
	返回数据说明：
		code:状态码
		msg:操作信息，用来记录失败信息
	返回数据示例
		正确 {"code":0,"msg":"ok"}
		错误 {"code":1007,"msg":"invalid parameters"}
		错误 {"code":8016,"msg":"The user has passed authentication"}

##指令：GET /certification/person/:loginname 查询个人认证信息(81)
<p style="color:red">需要修改的地方：loginname强两位字母为region值。管理员区分角色，如gz管理员仅能查询gz用户的认证信息。</p>
	说明
		【自己和管理员】查询个人认证信息
	输入参数说明：
		无
	Example Request：
		GET /certification/person/:loginname
		Accept: application/json;charset=UTF-8

	返回数据说明：
		code:状态码
		msg:操作信息，用来记录失败信息
		data：数据结果

		name：姓名
		tel：电话号码
        idNum：身份证号
        idPicUp:身份证正面id
		idPicDown:身份证反面id
		accountName：账户名称
		bankNum:银行账号
		bank:银行名称
		bankName:开户支行名称
		verifyPerson:审核人
		
	返回数据示例
		{"data":{"name":"郭立强",
				"tel":"8008208820",
				"idNum":"123131613215631",
				"idPicUp":"xxx",
				"idPicDown":"xxx",
				"accountName":"郭立强",
				"bankNum":"235315613",
				"bank":"xx银行"，
				"bankName":"xx支行"},"code":0,"msg":"ok"}

##指令：GET /certification/company/:loginname查询企业认证信息(81)
<p style="color:red">需要修改的地方：loginname强两位字母为region值。管理员区分角色，如gz管理员仅能查询gz用户的认证信息。</p>
	说明
		【自己和管理员】查询企业认证信息
	输入参数说明：
		无
	Example Request：
		GET /certification/company/:loginname
		Accept: application/json;charset=UTF-8

	返回数据说明：
		code:状态码
		msg:操作信息，用来记录失败信息
		data：数据结果

		name:企业名称
		address:公司地址
		kbisNum:营业执照编号
        kbisPic：营业执照扫描件id
		org：组织结构代码
        orgPic:组织结构代码扫描件id
		legalPerson：法人姓名
		legalPersonAddress：法人归属地
		legalPersonNum：法人身份证号
		legalPersonPicUp：法人身份证正面id
		legalPersonPicDown:法人身份证反面id
		person：联系人姓名
		personTel：联系人电话
		personEmail：联系人电子邮箱
		personNum:联系人身份证号
		personPicUp：联系人身份证正面id
		personPicDown：联系人身份证反面id
		accountName：账户名称
		bankNum:银行账号
		bank:银行名称
		bankName:开户支行名称
		verifyPerson:审核人
		
	返回数据示例
		{"data":{"name":"强大大有限公司",
				"address":"xxxxx",
				"kbisNum":"123131613215631",
				"kbisPic":"xxx",
				"org":"我是组织机构",
				"orgPic":"abc.jpg",
				"legalPerson":"zhangsan",
				"legalPersonAddress":"xx",
				"legalPersonNum":"111",
				"legalPersonPicUp":"xxx",
				"legalPersonPicDown":"xgx",
				"person":"郭立强",
				"personTel":"1232",
				"personEmail":"abc@abc.com",
				"personNum":"12315",
				"personPicUp":"xxx",
				"personPicDown":"xx",
				"accountName":"强大大有限公司",
				"bankNum":"235315613",
				"bank":"xx银行",
				"bankName":"xx支行"},"code":0,"msg":"ok"}

##指令：PUT /certification/inspection/:loginname 审核
<p style="color:red">需要修改的地方：loginname强两位字母为region值。管理员区分角色，如gz管理员仅能审核gz用户的认证信息。</p>
    说明：
		【管理员角色】审核实名认证
	输入参数说明：
		state：审核状态（1：审核通过，2：审核未通过）
		massage:审核不通过原因
		time：审核时间
	Example Request：
		PUT /certification/inspection/:loginname
		Content-Type: application/json;charset=UTF-8
		{
			"state":2,
			"massage":"balabala",
			"time":"2016-4-4"
		}	
	返回数据说明：
		code:状态码
		msg:操作信息，用来记录失败信息
	返回数据示例
		正确 {"code":0,"msg":"ok"}


##指令：GET /certification/inspections 查询审核信息列表
<p style="color:red">需要修改的地方：loginname强两位字母为region值。管理员区分角色，如gz管理员仅能查询gz用户的审核列表。</p>
    说明：
		【管理员角色】查询审核信息列表
		可根据登录名查询
	输入参数说明：
		loginname:登录名
		page:页码
		size：每页数量
		state：审核状态(3:审核通过，4：等待审核,5：审核不通过)
		type:用户类型(1:个人,2:企业)
		region: 用户登录来源
	Example Request：
		GET /certification/inspections?page=1&size=20&loginname=foo&state=3&type=2&sregion=a
		Content-Type: application/json;charset=UTF-8
	返回数据说明：
		code:状态码
		msg:操作信息，用来记录失败信息
		data:{
    	  total:总记录数
    	  results:数据集合  {
    	    loginname 登录名
    	    type:认证类别
		    state：认证状态
    	    createtime:提交时间
    	    time:审核时间
			sregion: 用户登录来源
		  }
		 }
	返回数据示例
		{"data":{"total":10,
				"results":[{"loginname":"abc@abc.com",
							"type":"1",
							"state":"2",
							"createtime":"2015-12-01 13:22",
							"time":"2015-12-01 13:22",
							"sregion":"GZ"}]},
							"code":0,"msg":"ok"}
		
		
##指令：POST /certification/upload 上传图片

    说明：
		【所有人】上传图片
	输入参数说明：
		pic：图片
	Example Request：
		POST /certification/upload
		Content-Type: application/json;charset=UTF-8
		pic:图片文件	
	返回数据说明：
		code:状态码
		msg:操作信息，用来记录失败信息
		picId:图片的id
	返回数据示例
		正确 {"code":0,"msg":"ok","picId":"xxx"}

##指令：GET /certification/download 查询图片
    说明：
		【所有人】查询图片
	输入参数说明：
		picId:图片id
	Example Request：
		GET /certification/download?picId=xxx
	返回数据说明：
		code:状态码
		msg:操作信息，用来记录失败信息
		pic:图片字节流
	返回数据示例
		图片文件
		正确 {"code":0,"msg":"ok"}

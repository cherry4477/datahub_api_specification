# API 列表
- GET /bill/:loginname/info 查看用户账户信息

- GET /bill/:loginname/detail 查看用户账单流水

- PUT /bill/:loginname/creditLimit 修改信用额度

- PUT /bill/:loginname/recharge 充值，提现

- PUT /bill/:loginname/trade/init 发起购买交易

- PUT /bill/:loginname/trade/commit 提交交易（交易结束）

- PUT /bill/:loginname/trade/cancel 取消交易

- PUT /bill/:loginname/refund 充值退款

- GET /bill/recharge 充值状态查询

- PUT /bill/message 接收回复报文

##指令：GET /bill/:loginname/info 查看用户账单信息
	说明：
		【自己，管理员】查看用户会员的相关信息
	输入参数说明：
		无
	Example Request：
		GET /bill/foo/info HTTP/1.1 
		Accept: application/json;charset=UTF-8
		Authorization: token
	返回数据说明：
		actualBalance:余额
		availableBalance:可用余额
		creditLimit:信用额度

	返回数据示例：

		{"data":{
			"actualBalance":1000,
			"availableBalance":800,
			"creditLimit":500
			},
		"code":0,"msg":"ok"
		}
##指令：GET /bill/:loginname/detail 查看用户账单流水
	说明：
		【自己，管理员】查看用户会员的相关信息
	输入参数说明：
		start_time:账单开始时间
		end_time:账单结束时间
		order:订单号
		trade_user:交易对方
		op_type:类型，-1:所有；1：充值；2：提现；3：扣年费；4：购买待生效；5：购买生效；6：购买失效；7：购买后退款，8：售出交易成功；9：售出交易生效；10：售出退款；11：充值退款；12：充值退款处理中；13：充值退款请求失败；14：充值退款失败;15:充值处理中；16：充值失败
		page:页码
		size：每页显示的记录数量
		
	Example Request：
		GET /bill/foo/detail?start_time=2015-09-02&end_time=2015-10-01&order=1&trade_user=xxx&op_type=1 HTTP/1.1 
		Authorization: token
		
	返回数据说明：
		Id:流水号
		orderId:订单号
		planId:套餐计划ID
		opTime:时间
		tradeItem:交易的item
		opType:类型，1：充值；2：提现；3：扣年费；4：购买待生效；5：购买生效；6：购买失效；7：购买后退款；8：售出交易成功；9：售出交易生效；10：售出退款；11：充值退款；12：充值退款处理中；13：充值退款请求失败；14：充值退款失败;15:充值处理中；16：充值失败
		tradeAmount:金额
		channel:渠道
		tradeUser:交易用户
		actualAmount:实际金额
		availableAmount：可用金额
		
	
	返回数据示例：
		{"data":{"total":13,
		 		"result":[
					{"date":"2016-01-15",
					 "detail":	[
						{"Id":"123",
						"orderId":"111",
						"planId":"11",
						"opTime":"2015-09-02",
						"tradeItem":"repo1_item",
						"opType":1,
						"tradeAmount":100,
						"channel":"网银",
						"tradeUser":"foo",
						"actualAmount":1000,
						"availableAmount":800}
						]
					}
					]},
			"code":0,
			"msg":"ok"
		}

#指令：PUT /bill/:loginname/recharge 充值，提现
	说明：
		【管理员】充值
	输入参数说明：
		sregion:用户登陆地
		order_id:订单
		amount:金额
		type:类型，1：充值；2：提现; 3：扣年费
		channel:渠道
		returnUrl:页面返回url
	Example Request：
		PUT /bill/foo/recharge?sregion=GZ HTTP/1.1 
		Accept: application/json;charset=UTF-8
		Authorization: token
		{
			"order_id":"recharge_11",
			"amount":"100",
			"type":1,
			"channel":"支付宝",
			"returnUrl":"balabala"		}

	返回数据说明：
		code:状态码（8007：余额不足）
		msg:操作信息，用来记录失败信息
		reqSignMsg:充值报文
	返回数据示例：
		{"code":0,"msg":"ok","reqSignMsg":"balabala"}
#指令：PUT /bill/:loginname/creditLimit
	说明：
		【管理员】修改用户信用额度
	输入参数说明：
		creditLimit:信用额度（可透支金额）
	Example Request：
		PUT /bill/foo/creditLimit HTTP/1.1 
		Accept: application/json;charset=UTF-8
		Authorization: token
		{
			"creditLimit":"1000"
		}
	返回数据说明：
		code:状态码
		msg:操作信息，用来记录失败信息
	返回数据示例：
		{"code":0,"msg":"ok"}
#指令：PUT /bill/:loginname/creditLimit
	说明：
		【管理员】修改用户信用额度
	输入参数说明：
		creditLimit:信用额度（可透支金额）
	Example Request：
		PUT /bill/foo/creditLimit HTTP/1.1 
		Accept: application/json;charset=UTF-8
		Authorization: token
		{
			"creditLimit":"1000"
		}
	返回数据说明：
		code:状态码
		msg:操作信息，用来记录失败信息
	返回数据示例：
		{"code":0,"msg":"ok"}
#指令：PUT /bill/:loginname/trade/init 发起购买交易
	说明：
		【管理员】购买交易，执行操作成功后，会生成一条记录，消费用户的消费记录，会扣取消费用户的可用余额
				如果购买金额不足，会提示余额不足，交易失败
				loginname：购买方
	输入参数说明：
		order_id:订单ID
		plan_id:套餐计划Id
		trade_item:交易item
		trade_amount:交易金额
		trade_user:卖方
	Example Request：
		PUT /bill/foo/trade/init HTTP/1.1 
		Accept: application/json;charset=UTF-8
		Authorization: token
		{
			"order_id":"trade_11",
			"plan_id":"111",
			"trade_item":"repo1_item1",
			"trade_amount":"100",
			"trade_user":"liuxy10@asiainfo.com"
		}
	返回数据说明：
		code:状态码（0：成功；8007：余额不足）
		msg:操作信息，用来记录失败信息
	返回数据示例：
		{"code":0,"msg":"ok"}

#指令：PUT /bill/:loginname/trade/commit 提交交易（交易结束）
	说明：
		【管理员】购买交易
			   如果交易成功（status=1），会扣取消费用户的实际余额，并增加销售用户的实际余额，等30天后自动将销售用户的可用余额增加
			   如果交易失败（status=2），会将消费用户的可用余额退还
			   若果交易生效（status=3），交易结束30天后执行的命令；将销售用户的可用余额增加
			   loginname：购买方
	输入参数说明：
		order_id:订单ID
		status：账单状态，1：交易成功；2:交易失败；3：交易生效
	Example Request：
		PUT /bill/foo/trade/commit HTTP/1.1 
		Accept: application/json;charset=UTF-8
		Authorization: token
		USER:admin
		{
			"order_id":"trade_11","status":"2"
		}
	返回数据说明：
		code:状态码
		msg:操作信息，用来记录失败信息
	返回数据示例：
		{"code":0,"msg":"ok"}
		
#指令：PUT /bill/:loginname/trade/cancel 取消交易
	说明：
		【管理员】取消交易，（交易成功30天内）会产生退款记录，将消费用户的可用余额和实际余额退还，销售用户的实际余额扣减
			     loginname：购买方
	输入参数说明：
		order_id:取消交易的订单ID
	Example Request：
		PUT /bill/foo/trade/cancel HTTP/1.1 
		Accept: application/json;charset=UTF-8
		Authorization: token
		{
			"order_id":"110"
		}
	返回数据说明：
		code:状态码
		msg:操作信息，用来记录失败信息
	返回数据示例：
		{"code":0,"msg":"ok"}
		
#指令：PUT /bill/:loginname/refund 充值退款
	说明：
		【用户自己】退还充值的金额
	输入参数说明：
		order_id:取消交易的订单ID
		returnUrl:返回url
		sregion:用户登录地
	Example Request：
		PUT /bill/foo/refund?sregion=GZ HTTP/1.1 
		Accept: application/json;charset=UTF-8
		Authorization: token
		{
			"order_id":"110",
			"returnUrl":"balabala",
			"sregion":"GZ"
		}
	返回数据说明：
		code:状态码
		msg:操作信息，用来记录失败信息
	返回数据示例：
		{"code":0,"msg":"ok"}

		
#指令：GET /bill/recharge 充值状态查询
	说明：
		【用户自己】
	输入参数说明：
		order_id:取消交易的订单ID
		
	Example Request：
		GET /bill/recharge?order_id=110 HTTP/1.1 
		Accept: application/json;charset=UTF-8
		Authorization: token
	返回数据说明：
		code:状态码
		msg:操作信息，用来记录失败信息
		result:充值状态（1:支付成功 2：查询失败 3：转账交易已受理 4：支付处理中 5：支付失败）
	返回数据示例：
		{"code":0,"msg":"ok","result":""}
		
#指令：PUT /bill/message 接收回复报文
	说明：
		【鸿支付】
	输入参数说明：
	Example Request：
		PUT /bill/message HTTP/1.1 
		Accept: application/json;charset=UTF-8
		Authorization: token
	返回数据说明：
		code:状态码
		msg:操作信息，用来记录失败信息
	返回数据示例：
		{"code":0,"msg":"ok"}

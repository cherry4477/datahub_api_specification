# API 列表

[GET] /select_labels

[POST] [PUT] [DELETE] /select_labels/:labelname

[POST] [PUT] [DELETE] [GET] /selects

----------

## 1 指令：[GET] /select_labels

说明
	
	【任意】返回精选栏目
		
Example Request：
	
	GET /select_labels HTTP/1.1 
	Accept: application/json
	

返回数据说明：
	
	labels  数据精选栏目名称数组

返回值示例

	{
	    [
            {
                "labelname": "CHINA3", "icon":"path1", "icon_hover":"path1_hover",
            },
            {
                "labelname": "CHINA2", "icon":"path2", "icon_hover":"path2_hover",
            },
            {
                "labelname": "CHINA", "icon":"path3", "icon_hover":"path2_hover",
            }
        ]
	}

## 2 指令：[POST] /select_labels/:labelname

说明
	
	【管理员】创建精选栏目

输入参数说明：
	
	order	  精选栏目排序（1-100， 排序越大越靠前）
	icon	  精选栏目图片路径
	icon_hover     精选栏目hover图片路径
		
Example Request：
	
	POST /select_labels/股市行情 HTTP/1.1 
	Accept: application/json
	Authorization: Token dcabfefb6ad8feb68e6fbce876fbfe778fb
	[
		{
			"order": 1,
			"icon":"path1"
		}
	]

Example Response：
	
	无

## 3 指令：[PUT] /select_labels/:labelname

说明
	
	【管理员】更新现有精选栏目的名称
	 	
	 newlabelname 	要改成的名字
	 order	  		精选栏目排序（1-100， 排序越大越靠前）
	 icon			精选栏目图片路径
	 icon_hover     精选栏目hover图片路径
	
输入参数说明：
	
	newlabelname 新精选栏目条目名称
	[URL]中labelname 为需要修改的名字
		
Example Request：
	
	PUT /select_labels/股市行情 HTTP/1.1 
	Accept: application/json
	Authorization: Token dcabfefb6ad8feb68e6fbce876fbfe778fb
	
	{
		{
			"order": 1,
			"icon":"path1"
			"newlabelname":"2015股市"
		}
	
	}

Example Response：
	
	无

## 4 指令：[DELETE] /select_labels/:labelname

说明

	【管理员】删除精选栏目
	
输入参数说明：
	
	无
		
Example Request：
	
	DELETE /select_labels/股市行情 HTTP/1.1 
	Accept: application/json
	Authorization: Token dcabfefb6ad8feb68e6fbce876fbfe778fb

Example Response：
	
	无

## 5 指令：[GET] /selects

说明
	
	【任意】返回精选内容，按照order排序
	【已登陆用户】返回该用户具有查询权限的private及public的Item
	
输入参数说明：
	
	select_labels  精选栏目名称
	若果不传，返回全部精选
	如果传，返回label类别的精选
			
Example Request：
	
	GET /selects?select_labels=chinamobile HTTP/1.1 
	Accept: application/json
	Authorization: Token dcabfefb6ad8feb68e6fbce876fbfe778fb

Example Response：

	{
	    "selects": [
	        {
	            "repname": "",
	            "itemname": ""
	        },
	        {
	            "repname": "",
	            "itemname": ""
	        },
	        {
	            "repname": "",
	            "itemname": ""
	        }
	    ]
	}


返回数据说明：
	
	selects： repname和itemname的数组

返回值示例
	
	    {
	        "selects": [
	            {	
	                "repname": "上海",
	                "itemname": "上海终端"
	            },
	            {
	                "repname": "北京",
	                "itemname": "北京终端"
	            },
	            {
	                "repname": "深圳",
	                "itemname": "深圳终端"
	            }
	        ],
	    }
	
## 6 指令：[POST] /selects/:repname/:itemname

说明
	
	【管理员】创建精选内容

输入参数说明：
	
	 select_labels[必选]：　	  	        精选栏目项
	 order[可选，默认为1，值越大排名越靠前]   	精选排序功能 
		
Example Request：
	
	POST /selects/:repname/:itemname HTTP/1.1 
	Accept: application/json
	Authorization: Token dcabfefb6ad8feb68e6fbce876fbfe778fb
	[
		{
			"select_labels":"广告信息"，
			"order":123
		}
	]

返回值示例
	
	无

## 7 指令：[PUT] /selects/:repname/:itemname

说明

	【管理员】更新精选内容

输入参数说明：
	
	 select_labels[必选]：　	                精选栏目项
	 order[可选，默认为1，值越大排名越靠前]   	精选排序功能 
		
Example Request：
	
	PUT /selects/:repname/:itemname HTTP/1.1 
	Accept: application/json
	Authorization: Token dcabfefb6ad8feb68e6fbce876fbfe778fb
	[
		{
			"select_labels":"广告信息"，
			"order":123
		}
	]

返回值示例

	无
	
## 8 指令：[DELETE] /selects/:repname/:itemname

说明

	【管理员】删除精选内容

输入参数说明：
	
	无
			
Example Request：
	
	DELETE /selects/:repname/:itemname HTTP/1.1 
	Accept: application/json
	Authorization: Token dcabfefb6ad8feb68e6fbce876fbfe778fb

Example Response：

	无

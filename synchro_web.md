# API 列表

- [GET] /synchroweb

----------

##1 指令：GET /synchroweb

说明：
	
	请求同步mysql数据
	
输入参数说明：
	
	type: 请求数据类型：noselects非精选，alldata所有数据（*必带）
	stype: 小分类，目前针对所有同步数据：public公有，private私有
	page: 表示第几页
	size: 表示每页几条数据

Example Request：
	
	GET /synchroweb?type=alldata&stype=public&page=5&size=8 HTTP/1.1 
	Accept: application/json
	Content-Type: application/json

返回数据说明：

    repname:repo名称
    itemname:item名称
    total:总数
    repuser:repo创建者
    itemuser:item创建者
    selectlabel:专题名称

返回数据示例

   【非精选】
	{
	    "code": 200,
	    "msg": "OK",
	    "data": {
	        "selects": [
	            {
	                "repname": "shanghai_soda",
	                "itemname": "climate_pcd_of_shanghai_soda"
	            },
	            {
	                "repname": "REPOSDFF",
	                "itemname": "item_private"
	            },
	            {
	                "repname": "Living_safety_of_beijing",
	                "itemname": "Civil_air_defense_construction_drawing_examination_institution_qualification_information"
	            }
	        ],
	        "total": 224
	    }
	}
	
   【所有数据】
	{
	    "code": 200,
	    "msg": "OK",
	    "data": {
	        "selects": [
	            {
	                "repuser": "yaxin@asiainfo.com",
	                "itemuser": "yaxin@asiainfo.com",
	                "repname": "Location_information",
	                "itemname": "Cell",
	                "selectlabel": "位置专题"
	            },
	            {
	                "repuser": "yaxin@asiainfo.com",
	                "itemuser": "yaxin@asiainfo.com",
	                "repname": "Internet_stats",
	                "itemname": "Music",
	                "selectlabel": "互联网专题"
	            },
	            {
	                "repuser": "bjjtgl@asiainfo.com",
	                "itemuser": "bjjtgl@asiainfo.com",
	                "repname": "Transportation_of_beijing",
	                "itemname": "Rail_transit_route",
	                "selectlabel": "北京公共专题"
	            }
	        ],
	        "total": 337
	    }
	}

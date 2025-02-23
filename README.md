# QLScript

注意事项: 

	如果发现账户名称不能被正确处理，请手动删除ql\scripts\CKName_cache.json 文件.
	
	另外某些账号如果服务器返回空有可能不会被正确处理，请知悉.
	
    ql.js 是jd_CheckCK.js和sendNotify.js的依赖,
	
	只要你使用了这两个脚本就一定保证放在同个文件夹里面.
	
	使用Ninjia要注意Extra.sh中把 cp sendNotify.js /ql/scripts/sendNotify.js 这一句删除，
	
	不然每次重启容器sendNotify.js都会被覆盖.

频道:
	
	https://t.me/ccwav_QLScript
	
	
脚本介绍:

1.jd_CheckCK.js

自动检测账号是否正常，不正常的自动禁用，正常的如果是禁用状态则自动启用.

当有自动禁用或自动启用事件发生才会发送通知.如果要每次都通知则需设定变量.

本脚本最后一次重试检测调用X1a0He写的接口加强验证.Thanks.

变量列表:
	
	显示正常CK:  export SHOWSUCCESSCK="true"

	永远通知CK状态:  export CKALWAYSNOTIFY="true"

	停用自动启用CK:  export CKAUTOENABLE="false"

	不显示CK备注:  export CKREMARK="false"
	
	服务器空数据等错误不触发通知:  export CKNOWARNERROR="true"

2.jd_bean_change.js

  京东资产变动 + 白嫖榜 + 京东月资产变动

注意: 

	如果你遇到TG报错，请参考https://github.com/ccwav/QLScript/issues/8
	
	如果你遇到Bark报错，请参考https://github.com/ccwav/QLScript/issues/7

变量列表:

(1) BEANCHANGE_PERSENT

    拆分通知
	
    例子 :  export BEANCHANGE_PERSENT="10"  总共有22个账号
	
	结果会分成3条推送通知，1~10为第一条推送，11~20为第二条推送，剩余的为第三条推送

(2)京东白嫖榜会独立通知
   
	即可以通过设置sendNotify的NOTIFY_GROUP_LIST加入“京东白嫖榜”单独控制通知组别.
	
	没有指定则默认通过组1的参数设定进行通知.

(3)BEANCHANGE_USERGP1和BEANCHANGE_USERGP2

	根据Pt_Pin的值进行分组通知，此功能搭配sendNotify的NOTIFY_CUSTOMNOTIFY功能可实现分账号通知不同群组功能.
	
	注意：分组通知会强制禁用BEANCHANGE_PERSENT变量!
	
	
	例子1 :  总共Test1~10 10个账号,变量设置如下:
	
	export BEANCHANGE_USERGP1="test1&test3&test4"
	
	export BEANCHANGE_USERGP2="test1&test5&test6"

	效果:通知会拆成3次进行发送:
	
		1) 通知标题为京东资产变动#1,内容为 test1 test3 test4 3个账号的资产
		
		   通知标题为京东白嫖榜#1,内容为 test1 test3 test4 3个账号的白嫖榜
		
		2) 通知标题为京东资产变动#2,内容为 test1 test5 test6 3个账号的资产
		
		   通知标题为京东白嫖榜#2,内容为 test1 test5 test6 3个账号的白嫖榜
		
		3) 通知标题为京东资产变动,内容为除了 test1 test3 test4 test5 test6 之外其他账号的资产
		
		   通知标题为京东白嫖榜,内容为除了 test1 test3 test4 test5 test6 之外其他账号的白嫖榜

	例子2 :  高级用法,需搭配发送通知脚本食用!
	
	总共Test1~10 10个账号,变量设置如下:
	
	export BEANCHANGE_USERGP1="test1&test2&test3&test4&test8"
	
	export NOTIFY_CUSTOMNOTIFY=["京东资产变动#1&组2&企业微信应用消息","京东资产变动&组2&pushplus","京东白嫖榜#1&组2&企业微信应用消息","京东白嫖榜&组2&pushplus"] 

	效果:通知会拆成2次进行发送:
	
		1) 通知标题为京东资产变动#1,内容为 test1 test2 test3 test4 test8 5个账号的资产,推送到组2的企业微信应用消息.
		
		   通知标题为京东白嫖榜#1,内容为 test1 test2 test3 test4 test8 5个账号的白嫖榜,推送到组2的企业微信应用消息.
			
		2) 通知标题为京东资产变动,内容为除了 test1 test2 test3 test4 test8 5个账号其他账号的资产,推送到组2的pushplus.
		
		   通知标题为京东白嫖榜,内容为除了 test1 test2 test3 test4 test8 5个账号其他账号的白嫖榜,推送到组2的pushplus.
	
	例子3 :  高级用法,需搭配发送通知脚本食用!
	
	使用BEANCHANGE_USERGP1后配置 NOTIFY_GROUP_LIST="京东资产变动#1"
	
	使用BEANCHANGE_USERGP2后配置 NOTIFY_GROUP3_LIST="京东资产变动#2"
	
	这样USERGP1 推送到组2，USERGP2推送到组3，剩下的推送到原始组别.
	
	(4) BEANCHANGE_DISABLECASH
	
	例子 :  export BEANCHANGE_DISABLECASH="true"  禁用领现金项目显示.
	
(4)BEANCHANGE_ENABLEMONTH	

	每月1号17点后如果执行资产查询，开启京东月资产变动的统计和推送.
	
	拆分通知和分组通知的变量都可以兼容.
	
	标题按照分组分别为 京东月资产变动#1 京东月资产变动#2 京东月资产变动
	
	开启 :  export BEANCHANGE_ENABLEMONTH="true"  	
		
	
	
3.sendNotify.js 

变量列表:

(1) NOTIFY_SKIP_LIST

    如果通知标题在此变量里面存在(&隔开),则用屏蔽不发送通知.
	(PS: Ningjia 作者写的功能，继承过来.)
	
    例子 :  export NOTIFY_SKIP_LIST="京东CK检测&京东资产变动"
	
(2) NOTIFY_GROUP_LIST 和 NOTIFY_GROUP3_LIST

    如果通知标题在此变量里面存在(&隔开),则用第2/3套推送变量进行配置.
	
	(PS:例子使用了企业微信的变量QYWX_AM,实际是所有推送变量后加2/3都会有效.)
	
    例子 :  export NOTIFY_GROUP_LIST="京东资产变动"
			export NOTIFY_GROUP3_LIST="Ninja 运行通知&京东CK检测"
	
	假如企业微信配置了QYWX_AM和QYWX_AM2和QYWX_AM3,则执行京东资产变动时会推送到QYWX_AM2配置的企业微信.
	
	Ninja 运行通知和京东CK检测会推送到QYWX_AM3配置的企业微信.
	
(3) SHOWREMARKTYPE

	例子: 账号名:ccwav  别名:ccwav的别名  Remark: 代码玩家
	
	export SHOWREMARKTYPE="1"    效果是 :  账号名称：代码玩家
	
    export SHOWREMARKTYPE="2"    效果是 :  账号名称：ccwav的别名(代码玩家)
	
    export SHOWREMARKTYPE="3"    不做处理,效果是 :  账号名称：ccwav   
	
	export SHOWREMARKTYPE="4"    效果是 :  账号名称：ccwav(代码玩家)
	
(4) NOTIFY_SKIP_REMARK_LIST 

	单独指定某些脚本不做REMARK处理
	
	(PS:京东CK检测我加了处理Remark，所以最好是加上不处理)
	
	例子 :  export NOTIFY_SKIP_REMARK_LIST="京东CK检测"  

(5) NOTIFY_COMPTOGROUP2 和 NOTIFY_NOREMIND

	用法1 : export NOTIFY_COMPTOGROUP2="true"	

		    东东农场 东东萌宠 京喜工厂 汪汪乐园养joy 这三个任务接收到产品可以兑换通知时推送到群组2.
			
	用法2 : export NOTIFY_COMPTOGROUP2="东东农场&东东萌宠&京喜工厂&汪汪乐园养joy"	

		    东东农场 东东萌宠 京喜工厂 汪汪乐园养joy的兑换通知推送到群组2,可自行删减.		
			
	用法3 : export NOTIFY_NOREMIND="东东农场&东东萌宠&京喜工厂&汪汪乐园养joy"	

		    屏蔽东东农场 东东萌宠 京喜工厂 汪汪乐园养joy的兑换通知,可自行删减.		
	
	群组2的介绍:假如企业微信配置了QYWX_AM和QYWX_AM2,则发送兑换通知时会推送到QYWX_AM2配置的企业微信.
	
	(PS:例子使用了企业微信的变量QYWX_AM,实际是所有推送变量后加2都会有效.)

(6) NOTIFY_NOCKFALSE

	屏蔽任务脚本的ck失效通知

	例子 :  export NOTIFY_NOCKFALSE="true"	
	
	        执行所有脚本时，如果有单独推送CK失效的请求也不会推送失效通知

(6) NOTIFY_AUTHOR	
		
	例子 :  export NOTIFY_AUTHOR="测试人"
			
			通知底部显示 本通知 By 测试人

(7) NOTIFY_NOLOGINSUCCESS

	例子 :  export NOTIFY_NOLOGINSUCCESS="true" 
			
			屏蔽青龙登陆成功通知，登陆失败不屏蔽.

(8) NOTIFY_CUSTOMNOTIFY 
	
	自定义脚本通知，可以指定通知脚本组别跟通知类型.
	
	格式为 脚本名称&推送组别&推送类型
	
	支持多个配置,通知类型关键字如下,可自行删减:
	
	Server酱&pushplus&Bark&TG机器人&钉钉&企业微信机器人&企业微信应用消息&iGotNotify&gobotNotify	
	
	例子 :  export NOTIFY_CUSTOMNOTIFY=["京东资产变动&组1&Server酱&Bark&企业微信应用消息","京东白嫖榜&组2&钉钉&pushplus"] 
	
	效果是: 京东资产变动推送到组1且只使用Server酱,Bark,企业微信应用消息三种方式进行通知.
	
			京东白嫖榜推送到组2且只使用钉钉,pushplus两种方式进行通知.
	
(9) NOTIFY_CKTASK

	当接收到发送CK失效通知和Ninja 运行通知时候执行子线程任务.
	
	(jd_CheckCK.js 可替换成其他任意qinglong支持的脚本文件.)
	
	例子: export NOTIFY_CKTASK="jd_CheckCK.js"
	
	效果: 当CK失效发送通知时自动执行jd_CheckCK.js
	
		  当Ninja更新CK发送通知时自动执行jd_CheckCK.js

4.jd_speed_sign_Part1~jd_speed_sign_Part3

	简单粗暴的极速版的分任务版，将总ck数除以3后平均分配成三个任务同时执行.
	
	如果使用请务必禁用其他库的jd_speed_sign脚本.感谢jd_speed_sign原作者。
	
	例子 : 有24个ck，则Part1 执行1~8,Part2 执行9~16，Part3 执行17以后剩下的所有ck.

5.jd_zy_ddwj_Mod.js 和 jd_zy_ddwj_help.js （已失效）

	主要功能来源于Ariszy大佬，我这种小白,只是改下一些执行逻辑。
	
	jd_zy_ddwj_Mod是东东玩家任务,jd_zy_ddwj_help是东东玩家的内部互助.
	
	源项目地址:https://github.com/Ariszy/Private-Script/tree/master/JD/zy_ddwj.js

6.jd_priceProtect_Mod.js

	京东价格保护通知版
	
	仅仅是保价成功加上了通知，改了执行时间,没有什么技术含量...

青龙拉库命令:

	不包含sendNotify:

	ql repo https://github.com/ccwav/QLScript.git "jd_" "sendNotify.js|NoUsed" "ql.js"

	包含sendNotify:

	ql repo https://github.com/ccwav/QLScript.git "jd_" "NoUsed" "ql.js|sendNotify.js"

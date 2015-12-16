Smarty语法：
=======

**1.标签:**

	${}：数据引用
	#{}	:smarty逻辑标签
	%{}%：JAVA语句块
	@@{}:生成地址

**2.数据引用**

	${obj}	//obj为后台传入页面的任意对象，并且可以访问对象的方法及属性
	${string.raw()}	//直接输出string中的html效果，不转换特殊字符

**3.逻辑操作**

***		条件：***

	#{if 条件语句}
	#{/if}
	#{elseif 条件语句}
	#{/elseif}
	#{else}
	#{/else}
	
***		循环：***

	#{list items:列表对象, as:'单体'}
	#{/list}
	#{else}		//当list循环次数为0时，可以进入else分支
	#{/else}
	
	#{list items:0..100, as:'i'} //从0到100循环
	#{/list}
	
**4.语句块**

***		直接在%{}%中嵌入语句（例）：**

	%{ int index = 0; }%
	#{list items:normalSenarios, as:'senario'}
	%{ Object rsltDecks = normalSenarioDecks.get(index); }%
	...
	#{/list}
	
**5.生成地址**

***HTML:***

	@@{Battle.event_show(superBossSenario.id, senario_type)
	
***JS:***

	var reward_list = #{jsAction @@Battle.event_reward_list(':page', ':event_id', ':reward_kind', ':reward_id', ':date') /};
	var url = reward_list({
		page: page,
		event_id: event_id,
		reward_kind: reward_kind,
		reward_id: 0,
		date: selectDateVal
	});
	
**6.引用静态文件**

	${play.configuration.getProperty('static_content_base') + '/public/files/文件名'}
	※static_content_base为application.conf中的设定内容
	
	
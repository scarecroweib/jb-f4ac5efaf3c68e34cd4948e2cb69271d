Smarty语法：
=======

**1.标签:**

	${}：数据引用
	#{}	:smarty逻辑标签
	%{}%：JAVA语句块
	@@{}:生成地址
	@{}: 输出当前工程下相对路径的地址(例如：@{'/public/test.png'}即为http://localhost:9000/pubic/test.png)
	*{ 注释内容 }*：注释语句，可换行

**2.数据引用**

	${obj}	//obj为后台传入页面的任意对象，并且可以访问对象的方法及属性
	${string.raw()}	//直接输出string中的html效果，不转换特殊字符
	${string.nl2br()}	//转换字符串中的回车为<br>
	${list.size().pluralize()}	//自动判断是否显示单词中表示复数的s
	${list[-1]}	//-1为获得列表中最后一个对象
	${comment.postedAt.format('dd MMM yy')}	//格式化日期

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
	
***#{}中***

	#{form @Application.postComment(post.id)}
	#{/form}
	
**6.引用静态文件**

	${play.configuration.getProperty('static_content_base') + '/public/files/文件名'}
	※static_content_base为application.conf中的设定内容
	
**7.Smarty标签***

	#{form @Application.postComment(post.id)}	//Form标签，用来定义一个HTML的Form router中如果定义为了POST的话，则生成一个POST的Form
	#{/form}
	
常用代码：
=====

##加载其他页面

	//加载一个main.html页面，并且设置一个title变量
	#{extends 'main.html' /}	
	#{set title:'Home' /}	

/yabe/app/views/main.html

	<!DOCTYPE html>
	<html>
	  <head>
	    <title>#{get 'title' /}</title>			//取得页面设置的变量
	    <meta charset="${_response_encoding}">
	    <link rel="stylesheet" media="screen"
	      href="@{'/public/stylesheets/main.css'}">
	    #{get 'moreStyles' /}
	    <link rel="shortcut icon" type="image/png"
	      href="@{'/public/images/favicon.png'}">
	    <script type="text/javascript" charset="${_response_encoding}"
	      src="@{'/public/javascripts/jquery-1.5.2.min.js'}"></script>
	    #{get 'moreScripts' /}
	  </head>
	  <body>
	    #{doLayout /}  							//加载页面内容
	  </body>
	</html>

##加载自定义tag

##tags定义：/app/views/tags/tag_name.html
	
	例：
	*{ Display a post in one of these modes: 'full', 'home' or 'teaser' }*
 
	<div class="post ${_as == 'teaser' ? 'teaser' : ''}">
	    <h2 class="post-title">
	        <a href="#">${_post.title}</a>
	    </h2>
	    <div class="post-metadata">
	        <span class="post-author">by ${_post.author.fullname}</span>,
	        <span class="post-date">${_post.postedAt.format('dd MMM yy')}</span>
	        #{if _as != 'full'}
	            <span class="post-comments">
	                &nbsp;|&nbsp; ${_post.comments.size() ?: 'no'} 
	                comment${_post.comments.size().pluralize()}
	                #{if _post.comments}
	                    , latest by ${_post.comments[-1].author}
	                #{/if}
	            </span>
	        #{/if}
	    </div>
	    #{if _as != 'teaser'}
	        <div class="post-content">
	            <div class="about">Detail: </div>
	            ${_post.content.nl2br()}
	        </div>
	    #{/if}
	</div>
	 
	#{if _as == 'full'}
	    <div class="comments">
	        <h3>
	            ${_post.comments.size() ?: 'no'} 
	            comment${_post.comments.size().pluralize()}
	        </h3>
	        
	        #{list items:_post.comments, as:'comment'}
	            <div class="comment">
	                <div class="comment-metadata">
	                    <span class="comment-author">by ${comment.author},</span>
	                    <span class="comment-date">
	                        ${comment.postedAt.format('dd MMM yy')}
	                    </span>
	                </div>
	                <div class="comment-content">
	                    <div class="about">Detail: </div>
	                    ${comment.content.escape().nl2br()}
	                </div>
	            </div>
	        #{/list}
	        
	    </div>
	#{/if}
	
##调用方法

	#{tag_name 参数1:数据1, 参数2:数据2 /}	//在tag中通过   _参数1 来获取传入的数据
	
	

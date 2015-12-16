models
======

##类标记

	@Entity		//说明该类通过JPA Entity管理
	@Table(name= "CcUser")	//默认情况下，DB名与Class名相关，当不相同时通过此标记说明
	
	@OnApplicationStart		//程序启动时执行，一般为Job类
	
##字段标记
	@Id			//说明主键，此字段为JPA必须
	
	@Lob		//说明此字段是一个large text
	
	@Type(type = “org.hibernate.type.TextType”)	//指定字段类型
	
	@ManyToOne	//说明此字段是另外一个类型对象（即关联），指定多对一的关系(N:1) 检索时同时检索关联对象
	
	@OneToMany(mappedBy="post", cascade=CascadeType.ALL)	//与ManyToOne相对应的一对多关系，即当前字段为多个类型对象列表，并且依赖于post字段，检索时同时检索关联列表
    public List<Comment> comments;
 
	@ManyToMany(cascade=CascadeType.PERSIST) //多对多的关系
	
#####对象到对象的级联关系（cascade）
	CascadeType.REFRESH:级联刷新，当多个用户同时作操作一个实体，为了用户取到的数据是实时的，在用实体中的数据之前就可以调用一下refresh（）方法！
	CascadeType.REMOVE:级联删除，当调用remove（）方法删除Order实体时会先级联删除OrderItem的相关数据！
	CascadeType.MERGE:级联更新，当调用了Merge（）方法，如果Order中的数据改变了会相应的更新OrderItem中的数据，
	CascadeType.ALL:包含以上所有级联属性。
	CascadeType.PERSIST:级联保存，当调用了Persist（） 方法，会级联保存相应的数据 		
##In和having的用法例子

	查询Post中包含所有tags的数据
	Post.find(
                "select distinct p from Post p join p.tags as t where t.name in (:tags) group by p.id, p.author, p.title, p.content,p.postedAt having count(t.id) = :size"
        ).bind("tags", tags).bind("size", tags.length).fetch();
    
Controller
======

##方法标记
	
	@Before		//所有请求处理前执行

###Validation
#####*使用validation的情况下，当出现错误时，会自动向页面返回一个名为errors的数组

	#{ifErrors}	//smarty中的validation错误标签
	#{/ifErrors}

	@Required 为空检查
	@Required(message="Author is required")  出错生输出自定义消息
	例：
	public static void postComment(Long postId, @Required String author, @Required String content) {
	    Post post = Post.findById(postId);
	    if (validation.hasErrors()) {
	        render("Application/show.html", post);
	    }
	    post.addComment(author, content);
	    show(postId);
	}
	
	动态检查：如下，比输两个字符串，不相同时输出错误信息
	validation.equals(
        code, Cache.get(randomID)
    ).message("Invalid code. Please type it again");
	
##常用方法

	Play.configuration.getProperty("blog.title")	//获取配置文件中的配置信息
	
##flash信息
	Controller:
		flash.success("Thanks for posting %s", author);

	Smarty:
		#{if flash.success}
	    <p class="success">${flash.success}</p>
		#{/if}
		
##创建验证码

	public static void captcha(String id) {
    	Images.Captcha captcha = Images.captcha();//创建验证码 
	    String code = captcha.getText("#E4EAFD");//获取验证码，并设置颜色
		Cache.set(id, code, "10mn");//放入cache中，保存10分钟
	    renderBinary(captcha);		//输出到页面
	}
	
##创建UUID

	Codec.UUID();
	




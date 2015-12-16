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
 
    
Controller
======

##方法标记
	
	@Before		//所有请求处理前执行

###Validation

	#{ifErrors}	//smarty中的validation错误标签
	#{/ifErrors}

	@Required 为空检查
	例：
	public static void postComment(Long postId, @Required String author, @Required String content) {
	    Post post = Post.findById(postId);
	    if (validation.hasErrors()) {
	        render("Application/show.html", post);
	    }
	    post.addComment(author, content);
	    show(postId);
	}
	
##常用方法

	Play.configuration.getProperty("blog.title")	//获取配置文件中的配置信息
	
##flash信息
	Controller:
		flash.success("Thanks for posting %s", author);

	Smarty:
		#{if flash.success}
	    <p class="success">${flash.success}</p>
		#{/if}
		
	
# jb-f4ac5efaf3c68e34cd4948e2cb69271d （jinbinchuangye-crm-doc）

说明：
各种手顺及文档。

＝＝＝＝＝＝＝＝＝＝＝＝＝＝＝＝＝＝＝＝＝

开发环境：

1.JDK1.8.0_66

2.mySQL 5.6.19

3.memcached

4.playframework 1.4.X

5.IDE:Eclipse

＝＝＝＝＝＝＝＝＝＝＝＝＝＝＝＝＝＝＝＝＝

play framework环境：

1.代码路径

https://github.com/scarecroweib/play1.git

2.使用版本 1.4.X

3.先fork到自己名下，然后clone到本地

4.选择1.4分支

5.Ant编译 play1下的framework/build.xml

6.将play1所在路径设置到PATH中，以便使用play命令。

play framework 官网：https://www.playframework.com/documentation/1.3.x/home

注：官方最新版本为2.5.X,但由于是基于Scala开发，所以JAVA环境时继续使用1.X版

另：Oracle目前已停止对JDK8以下版本支持，所以为了能在JDK8环境下编译，所以使用play1中的最新版本1.4.X

＝＝＝＝＝＝＝＝＝＝＝＝＝＝＝＝＝＝＝＝＝

Playframework命令：

1.创建工程：

	play new 工程名

2.IDE支持

	play eclipsify 工程名

在eclipse中导入工程，此时工程目录下有一个eclipse目录，目录下有一个以工程名命名的.launch文件。

打开文件，删除最后一行类似以下runjdwp的参数

-Xrunjdwp:transport=dt_socket,address=8000,server=y,suspend=n

右键点击该文件，运行。

访问http://localhost:9000

这里因为在页面中有访问google的内容，因此最好事先在本地的hosts中把www.google.com的域名定向到本地。




AccountingForm[N[Table[CDF[PoissonDistribution[4], i], {i, 1, 9}], 20]*2^32]

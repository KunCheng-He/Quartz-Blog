# 0-IDEA创建项目

+ 下载模板的地址可以使用阿里云：http://start.aliyun.com
+ 创建完项目后下载 `jar` 包失败，自行新建 `settings.xml` 进行覆盖，内容如下：

![[Pasted image 20230918221223.png]]

```xml
<mirror>
	  <id>alimaven</id>
	  <mirrorOf>central</mirrorOf>
	  <name>aliyun maven</name>
	  <url>http://maven.aliyun.com/nexus/content/repositories/central/</url>
</mirror>
```

+ SpringBoot程序优点
	+ 起步依赖（简化依赖配置）
	+ 自动配置（简化常用工程相关配置
	+ 辅助功能（内置服务器，…）

+ starter
	+ SpringBoot中常见项目名称，定义了当前项目使用的所有依赖坐标，以达到减少依赖配置的目的
+ parent
	+ 所有SpringBoot项目要继承的项目，定义了若干个坐标版本号（依赖管理，而非依赖），以达到减少依赖冲突的目的
	+ spring-boot-starter-parent各版本间存在着诸多坐标版本不同
+ 实际开发
	+ 使用任意坐标时，仅书写GAV中的G和A,V由SpringBoot提供，除非SpringBoot未提供对应版本V
	+ 如发生坐标错误，再指定Version(要小心版本冲突)

+ SpringBoot的引导类是Boot工程的执行入口，运行main方法就可以启动项目
+ SpringBoot.工程运行后初始化Spring容器，扫描引导类所在包加载bean

+ 内置服务器
	+ tomcat(默认)：apache出品，粉丝多，应用面广，负载了若干较重的组件
	+ jetty：更轻量级，负载性能远不及tomcat
	+ undertow：undertow,负载性能勉强跑赢tomcat

+ springboot 默认的配置文件路径 `resources/application.properties` ，这个后缀文件在 `java` 中学过，是一个 `k-v` 结构的 `Map`
+ 这个配置文件的格式在 SpringBoot 中有三种：`properties` 、 `yml` 主流、 `yaml` （三种配置文件的优先级从高到低）

+ 使用@ConfigurationProperties注解绑定配置信息到封装类中
+ 封装类需要定义为Spring管理的bean,否则无法进行属性注入


https://www.bilibili.com/video/BV15b4y1a7yG?p=43

中文文档
https://springdoc.cn/spring-boot/index.html





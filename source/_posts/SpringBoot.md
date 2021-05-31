---
title: SpringBoot 
date: 2021-05-31 11:42:36
tags: 
	- Java
	- SpringBoot
categories: 
	- SpringBoot
	
cover: /images/banners/VCG41N1169192820.jpg
---
[toc]



# Spring boot 2

### 一、Boot入门

**SpringBoot2核心技术与响应式编程: https://www.yuque.com/atguigu/springboot**

#### 1、简介
- 简化Spring应用开发的一个框架；
- 整个Spring技术栈的一大整合；
- J2EE开发的一站式解决方案；


Spring Boot来简化Spring应用开发，约定大于配置，去繁从简，just run就能创建一个独立的，产品级别的应用

**背景:**

J2EE笨重的开发、繁多的配置、低下的开发效率、复杂的部署流程、第三方技术集成难度大。

**解决∶**

"Spring全家桶”时代。
Spring Boot >J2EE一站式解决方案Spring Cloud→分布式整体解决方案

User -> Spring Boot -> Spring

**优点**
+ 快速创建独立运行的Spring项目以及主流框架集成
+ 使用嵌入式的Servlet容器，应用无需打成WAR包
+ starters自动依赖与版本控制
+ 大量的自动配置，简化开发，也可修改默认值
+ 无需配置XML，无代码生成，开箱即用
+ 准生产环境的运行时应用监控
+ 与云计算的天然集成

#### 2、微服务
2014， martin fowler

微服务：架构风格（服务微化）

一个应用应该是一组小型服务；可以通过
HTTP的方式进行互通；

单体应用：ALL IN ONE
微服务：每一个功能元素最终都是一个可独立替换和独立升级的软件单元；

#### 3、环境配置
- jdk 1.8.0_40 
- Spring boot 2.4.3
- IDEA 2020.3
- Maven 3.6.3
- 给Maven的settigs.xml文件中的 </profiles>里加上如下配置

```xml
<profile>
  <id>jdk-1.8</id>
  <activation>
    <activeByDefault>true</activeByDefault>
    <jdk>1.8</jdk>
  </activation>
  <properties>
    <maven.compiler.source>1.8</maven.compiler.source>
    <maven.compiler.target>1.8</maven.compiler.target>
    <maven.compiler.compilerVersion>1.8</maven.compiler.compilerVersion>
  </properties>
</profile>
```
#### 4、Spring Boot HelloWorld
功能：
浏览器发送hello请求，服务器请求并处理，响应HelloWorld字符串

##### 4.1 初始化项目
根据该网址创建项目：https://blog.51cto.com/yaowusheng/2565318

不能使用RequestMapping等，在pom文件中添加如下 
```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-web</artifactId>
</dependency>

```

##### 4.2 编写相关的Controller、Service 

(注意，contrller类所在包的级别，要在主程序类下。否则找不到)

```java
package com.example.springbootdemo.controller;

import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.ResponseBody;

@Controller
public class HelloWorldController {
    @ResponseBody
    @RequestMapping("/hello")
    public String helloWorld() {
        return "hello";
    }
}

```
##### 4.3 打包成可执行的jar包
 IDEA操作步骤

在IDEA右侧，选择Maven-->展开自己的项目--》选择Lifecycle--》双击package
-->运行完成之后，在左边项目目录中展开target-->里面有个jar包，可以使用java -jar运行该jar包。
```xml
<!--    这个插件，可以应用打包成一个可执行的jar包-->
    <build>
        <plugins>
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
            </plugin>
        </plugins>
    </build>
```
#### 5、探究Hello World
##### 5.1 POM文件
###### 5.1.1 父项目
```xml
    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>2.4.3</version>
        <relativePath/> <!-- lookup parent from repository -->
    </parent>
    
    他的父项目是
    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-dependencies</artifactId>
        <version>2.4.3</version>
    </parent>
他来真正管理Spring Boot应用里面的所有依赖版本；
```

Spring Boot的版本仲裁中心；

以后我们导入依赖默认是不需要写版本；（没有在dependencies里面管理的依赖自然需要声明版本号）

###### 5.1.2 启动器
```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-web</artifactId>
</dependency>
```

**Spring-boot-starter-web**:

​	Spring-boot-starter:spring-boot 场景控制器：帮我们导入了web模块正常运行所依赖的组件 ；

Spring Boot将所有的功能场景都抽取出来 ，做成一个个的starters（启动器），只需要在项目里面引入 这些starter相关场景的所有依赖都会导入进行。要用什么功能就导入什么场景的启动器

##### 5.2 主程序类，主入口类

```java
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
/**
@SpringBootApplication来标注一个主程序类，说明这是一个Spring Boot应用
*/
@SpringBootApplication
public class SpringbootdemoApplication {

    public static void main(String[] args) {
        SpringApplication.run(SpringbootdemoApplication.class, args);
    }

}
  
```

**@SpringBootApplication** : Spring Boot应用标注在某个类上说明这个类是SpringBoot的主配置类，SpringBoot就应该运行这个类的main方法来启动SpringBoot应用；

```java
@Target({ElementType.TYPE})
@Retention(RetentionPolicy.RUNTIME)
@Documented
@Inherited
@SpringBootConfiguration
@EnableAutoConfiguration
@ComponentScan(
    excludeFilters = {@Filter(
    type = FilterType.CUSTOM,
    classes = {TypeExcludeFilter.class}
), @Filter(
    type = FilterType.CUSTOM,
    classes = {AutoConfigurationExcludeFilter.class}
)}
)
```

**@SpringBootConfiguration** : Spring Boot的配置类
	标注在某个类上，表示是一个Spring Boot的配置类；

​	**@Configuration**:配置类上来标注这个注解；

​		配置类------配置文件；配置类也是容器中的一个组件；@Component

**@EnableAutoConfiguration**:开启自动配置功能；

​	以前我们需要配置的东西，Spring Boot帮我们自动配置；**@EnableAutoConfiguration**告诉Spring Boot开启自动配置功能；这样自动配置才能生效；

```java
@AutoConfigurationPackage
@Import({AutoConfigurationImportSelector.class})
public @interface EnableAutoConfiguration {
```

**@AutoConfigurationPackage**: 自动配置包

​	@**Import**({AutoConfigurationImportSelector.class})；

​	Spring的底层注解@import，给容器中导入一个组件；导入的组件由AutoConfigurationPackages.Registrar.class；

**将主配置类（@SpringBootApplication标注的类）的所在包及下面所有子包里面的所有组件扫描到Spring容器；**

​	@Import(AutoConfigurationImportSelector.class) ;

​		给容器中导入组件？

​		**EnableAutoConfiguratioImportSelector**： 导入哪些组件的选择器；

​		将所有需要导入的组件以全类名的方式返回；这些组件就会被添加到容器中；

​		会给容器中导入非常多的自动配置类（xxxAutoConfiguration）;就是给容器中导入这个场景所需要的所有组件，并配置好这些组件；



有了自动配置类，免去了我们手动编写配置注入功能组件等工作；

​		SpringFactoriesLoader.loadFactoryNames(EnableAutoConfiguration.class, classLoader);

Spring Boot启动的时候从类路径下的META-INF/spring.factories中获取EnableAutoConfiguration指定的值 ,将这些值作为自动配置类导入到容器中，自动配置类就生效，帮我们进行自动配置工作；



J2EE的整体整合解决方案和自动配置都在**spring-boot-autoconfigure-2.4.3.jar**;

#### 6、使用Spring Initializer快速创建Spring Boot项目

根据该网址创建项目：https://blog.51cto.com/yaowusheng/2565318

IDE都支持使用Spring的项目创建向导快速创建一个Spring Boot项目；

选择我们需要的模块；向导会联网创建Spring Boot项目；

默认生成的Spring Boot项目；

- 主程序 已经写好，只需要写我们的逻辑
- resources文件夹中目录结构
  - static :保存所有的静态资源：js css images；
  - templates:保存所有的模板页面：（Spring Boot默认jar包使用嵌入式的Tomcat,默认不支持JSP页面）‘可以使用模板引擎（freemarker、thymeleaf）；
  - application.properties: Spring Boot应用的配置文件；可以修改一些默认设置；



### 二、Spring Boot配置

#### 2.1 配置文件

SpringBoot使用一个全局的配置文件，配置文件名是固定的;

**如果同时配置，properties优先yml**

- application.properties

- application.yml

  

  配置文件的作用: 修改SpringBoot自动配置的默认值;

  SpringBoot在底层都给我们自动配置好;

YAML（YAML Ain`t Markup Language）

	+ YAML A Markup Language：是一个标记语言
	+ YAML isn`t Markup Language：不是一个标记语言

标记语言：

	+ 以前的配置文件：大多都使用的是xxx.xml文件；
	+ YAML:以数据为中心，比json、xml更适合做配置文件；
	+ YAML:配置例子

```yaml
server:
	port: 8080
```

	+ XML:

```xml
<server>
	<port>8080</port>
</server>
```

#### 2.2、YAML语法

##### 2.2.1 基本语法

k:(空格)v:表示一对键值对（空格必须有);
以**空格**的缩进来控制层级关系;只要是左对齐的一列数据，都是同一个层级的

```yml
server:
	port: 8081
	path: /hello
```

属性和值也是大小写敏感;

##### 2.2.2 值的写法

###### 字面量︰普通的值（数字，字符串，布尔)
​	k: v: 字面直接来写;
​		字符串默认不用加上单引号或者双引号;

​			“” 双引号：不会转义字符串里面的特殊字符;特殊字符会作为本身想表示的意思

​				name: "zhangsan \n lisi	输出: zhangsan 换行 lisi

​			’:单引号:会转义特殊字符，特殊字符最终只是一个普通的字符串数据
​				name: "zhangsan \n lisi	输出: zhangsan in lisi

###### 2.2.3 对象、Map（属性和值）（键值对）：

​	k: v :在下一行来写对象的属性和值的关系； 注意缩进

​		对象还是k: v的方式

```yaml
server:
	port: 8081
```

行内写法：

```
server: (port: 8081, path: /test)
```

###### 数组（List、Set）:

用 - 值表示数组中的一个元素

```yaml
pets:
- cat
- dog
- pig
```

行内写法

```yaml
pets: [cat, dog, pig]
```

#### 2.3 配置文件值注入

##### 2.3.1 配置文件 xx.yaml

```yaml
person:
  name: xiaozhu
  isDog: true
  age: 18
  data: 2020/12/1
  map: {k1: yihao, k2: erhao}
  list:
    - qqq
    - aaa
    - ccc
  dog:
    name: jingmao
    remarks: ni shi jing mao 
```

javaBean:

```java
/*
* 将配置文件中配置的每一个属性值，映射到这个组件中
* @ConfigurationProperties : 告诉Spring Boot将本类中的所有属性和配置文件中相关的配置进行绑定；
* prefix = "person" ，配置文件中哪个下面的所有属性一一映射*/
@Component
@ConfigurationProperties(prefix = "person")
public class Person {
    String name;
    boolean isDog;
    Integer age;
    Date data;
    Map<String, Object> map;
    List<Object> list;
    Dog dog;
```

导入配置文件处理器，编写配置就会有提示

```xml
<!--   导入配置文件处理器，配置文件进行绑定就会有提示     -->
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-configuration-processor</artifactId>
    <optional>true</optional>
</dependency>
```

##### 2.3.2使用properties，其他配置相同，properties文件如下

```properties
person.name=张三
person.age=19
person.list=a,b,c
person.map.k1=aaa
person.map.k2=bbb
person.dog.name=xiaoha
person.dog.remarks=nihao
# 如果遇到中文编码问题，在settings-->file Encodings--> Properties Files --> 选择UTF-8加上勾选Transparent native-to-ascii conversion
```

![1614492402407](D:\data\projects\gitPro\study-notes\SpringBoot\images\1614492402407.png)

##### 2.3.3 @Value获取值和@ConfigurationProperties获取值比较

|                      | @ConfiggurationProperties |   @Value   |
| :------------------: | :-----------------------: | :--------: |
|         功能         | 批量注入配置文件中的属性  | 一个个指定 |
| 松散绑定（松散语法） |           支持            |   不支持   |
|         SpEL         |          不支持           |    支持    |
|    JSR303数据校验    |           支持            |   不支持   |
|     复杂类型封装     |           支持            |   不支持   |

配置文件yml还是properties他们都能获取到值；

如果说，我们只是在某个业务逻辑中需要获取一下配置文件中的某项值，使用@Value

如果说，我们专门编写了一个javaBean来和配置文件进行映射，我们就直接使用@ConfigurationProperties;

##### 2.3.4 @PropertySource& @ImportResource

@**PropertySOurce**加载指定的配置文件；

```java
/*
* 将配置文件中配置的每一个属性值，映射到这个组件中
* @ConfigurationProperties : 告诉Spring Boot将本类中的所有属性和配置文件中相关的配置进行绑定；默认全局文件获取值
* prefix = "person" ，配置文件中哪个下面的所有属性一一映射*/
@PropertySource(value = {"classpath:person.properties"})
@Component
@ConfigurationProperties(prefix = "person")
public class Person {
    String name;
    boolean isDog;
    Integer age;
    Date data;
    Map<String, Object> map;
    List<Object> list;
    Dog dog;
```

**@ImportResource**:导入Spring的配置文件，让配置文件里面的内容生效；

Spring Boot里面没有Spring的配置文件，我们自己编写的配置文件，也不能自动识别；

想让Spring的配置文件生效；**@ImportResource**标注在一个配置类上

```java
@ImportResource(locations = {"classpath:beans.xml"})
导入spring的配置文件
```

SpringBoot推荐给容器中添加组件的方式：推荐使用全注解方式

1、配置类====Spring配置文件

2、使用@Bean给容器中添加组件

```java
/*
    @Configuration:指明当前类是一个配置类；就是来替代之前的Spring配置文件
    在配置文件中用<bean><bean/>标签添加组件
 */
@Configuration
public class MyAppConfig {
    // 将方法的返回值添加到容器中，容器中的这个组件默认的id就是方法名
    @Bean
    public HelloService helloService() {
        System.out.println("helloService已添加");
        return new HelloService();
    }
}
```

#### 2.4、配置文件占位符

##### 2.4.1、随机数

```properties
random.value
random.int
${random.long}
random.int(10)
{random.int[1024, 65536]}
```

##### 2.4.2、点位符获取之前配置的值，如果没有可以用：号来指定默认值

```properties
person.name=张三${random.uuid}
person.age=${random.int}
person.list=a,b,c
person.map.k1=aaa
person.map.k2=bbb
person.dog.name=${person.wq: zhu}dog
```

#### 2.5、Profile,测试环境，正式环境配置

##### 2.5.1、多Profile文件

我们在主配置文件编写的时候，文件名可以是： application-{profile}.properties/yml

![image-20210228152740784](D:\data\projects\gitPro\study-notes\SpringBoot\images\image-20210228152740784.png)

##### 2.5.2、yml支持多文档块方式

```yaml
spring:
  profiles:
    active: dev
    
---
server:
  port: 8020
spring:
  profiles: dev
  
---
server:
  port: 8030
spring:
  profiles: prod

```



##### 2.5.3、激活指定profile

1、

```properties
spring.profiles.active=dev
```

2、命令行：

```properties
java -jar ***.jav --spring.profiles.active=dev
可以直接在测试的时候，配置传入命令行参数
```



![image-20210228154415200](D:\data\projects\gitPro\study-notes\SpringBoot\images\image-20210228154415200.png)

3、虚拟机参数

```properties
-Dspring.profiles.active=dev
```

![image-20210228154312645](D:\data\projects\gitPro\study-notes\SpringBoot\images\image-20210228154312645.png)

#### 2.6、配置文件加载位置

. spring boot启动会扫描以下位置的application.properties或者application.yml文件作为Spring boot的默认配置文件

```properties
# 配置项目的访问路径
server.context-path=/boot02
```

- file:./config/
- file:./
- classpath:/config/
- classpath:/
- 一以上是按照**优先级**从**高到低**的顺序，所有位置的文件都会被加载，**高优先级配置内容会覆盖低优先级配置内容**。
- SpringBoot会从四个位置全部加载主配置文件；互补配置；
- **我们也可以通过配置spring.config.location来改变默认配置；**

项目打包好以后，我们可以使用命令行参数的形式，启动项目的时候来指定配置文件的新位置；指配置文件和默认加载的这些配置文件共同起作用形成互补配置；

#### 2.7、外部配置加载顺序

SpringBoot民可以从以下位置加载配置；优先级从高到低；高优先级的配置覆盖低优先级的配置

1. 命令行参数

```java
java -jar *.jar --server.port=8087 --server.context-path=/abc
// 多个配置用空格分开；--配置项=值
```

 2. 来自java:comp/env的JNDI属性

 3. java系统属性（System.getProperties()）

 4. 操作系统环境变量

 5. RandomValuePropertySource配置的random.*属性值

    **由jar包外向jar包内进行寻找；**

    **优先加载带profile**

 6. jar包外部的application-{profile}.properties或application.yml(带spring.profile)配置文件

 7. jar包内部的application-{profile}.properties或application.yml(带spring.profile)配置文件

    **再来加载不带profile**

 8. jar包外部的application.properties或application.yml(不带spring.profile)配置文件

 9. jar包内部的application.properties或application.yml(不带spring.profile)配置文件

 10. @Configuration注解类上的@PropertySource

 11. 通过SpringApplication.setDefaultProperties指定的默认属性

#### 2.8、自动配置原理

配置文件到底能写什么？怎么写？自动配置原理；

配置文件能配置的属性参照 

自动配置原理：

1）、SpringBoot启动的时候加载主配置类，开启了自动配置功能@EnableAutoCOnfiguration

2）、@EnableAutoConfiguration作用；

​	利用EnableAutoConfigurationImportSelector给窗口中导入一些组件？

​	可以插件selectImports()方法的内容；

​	List<String> configurations = getCandidateConfigurations(annotationMetadata, attributes);

### 三、Spring Boot与日志

### 四、Spring Boot与Web开发

### 五、Spring Boot与Docker

### 六、Spring Boot与数据访问

### 七、Spring Boot启动配置原理

### 八、Spring Boot自定义starters

### 九、Spring Boot与缓存

### 十、Spring Boot与消息

### 十一、Spring Boot与检索

### 十二、Spring Boot与任务

### 十三、Spring Boot与安全

### 十四、Spring Boot与分布式

### 十五、Spring Boot与开发热部署

### 十六、Spring Boot与监控管理
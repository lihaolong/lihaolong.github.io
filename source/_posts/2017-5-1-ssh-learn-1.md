---
title: SSH框架学习之旅（一）：框架搭建
date: 2017-5-1
categories:
- programming
tags:
- framework
---
## 1.导入相关jar包
### （1）下载并导入spring相关jar包
进入[http://repo.spring.io/release/org/springframework/spring/](http://repo.spring.io/release/org/springframework/spring/)下载相应版本的zip文件，解压后得到jar包。复制jar包到WEB-INF/lib并build path对应的jar包。

### （2）下载并导入Struts相关jar包

进入[http://struts.apache.org/download.cgi#struts25101](http://struts.apache.org/download.cgi#struts25101)下载相应版本的zip文件，解压后得到jar包。复制jar包到WEB-INF/lib并build path对应的jar包。
 
### （3）下载并导入hibernate相关jar包

进入[http://hibernate.org/orm/downloads/](http://hibernate.org/orm/downloads/)下载相应版本的zip文件，解压后得到jar包。复制jar包到WEB-INF/lib并build path对应的jar包。

## 2.spring相关配置

### （1）在web.xml配置spring

```

    <context-param>
    <param-name>contextConfigLocation</param-name>
    <param-value>classpath:applicationContext.xml</param-value>
    </context-param>
    <listener>
    <listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
    </listener>
```

## 3.Struts相关配置

### （1）在web.xml配置struts

```

    <filter>
    <filter-name>struts2</filter-name>
    <filter-class>org.apache.struts2.dispatcher.ng.filter.StrutsPrepareAndExecuteFilter</filter-class>
    </filter>
    <filter-mapping>
    <filter-name>struts2</filter-name>
    <url-pattern>/*</url-pattern>
    </filter-mapping>
```

### （2）在Struts.xml中配置Struts

```
    <struts>
	<constant name="struts.i18n.encoding" value="UTF-8"></constant>
	<constant name="struts.objectFactory" value="spring"></constant>
	<package>
		<action>
			<result></result>
		</action>
		<action>
			<result></result>
		</action>
	</package>
    </struts>
```

## 4.hibernate相关配置

### （1）在applicationContext.xml中配置hibernate

```

    <bean name="sf"  class="org.springframework.orm.hibernate3.LocalSessionFactoryBean">
        <property name="dataSource" ref="myDataSource" />
        <property name="mappingResources">
            <list>
                <value></value>
                <value></value>
            </list>
        </property>
        <property name="schemaUpdate">  
            <value>true</value>  
        </property>
        <property name="hibernateProperties">  <value>hibernate.dialect=org.hibernate.dialect.MySQLDialect
                hibernate.show_sql=true
                hbm2ddl.auto=update
            </value>
        </property>
    </bean> 
```

## 5.Tips

1.连接数据库还需导入jdbc的jar包，单元测试需要Junit的jar包。<br>
2.注意配置文件的编写，任何地方有错均不能搭建成功。<br>

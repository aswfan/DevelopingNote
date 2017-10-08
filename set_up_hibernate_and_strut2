#### 1. org.hibernate.hql.ast.QuerySyntaxException：XXX is not mapped ####
##### hql: from User where user_name =? and password =?
#####
**可能原因：**

· from后面应该是是实体类名(区分大小写)，而不是表名，where后面应该是实体类中的变量名，而不是数据库中表单的column name

· hibernate中配置文件中没加入相应的映射文件<mapping resource="...">

#### 2. WARN: HHH000277: Could not bind factory to JNDI
org.hibernate.engine.jndi.JndiException: Error parsing JNDI name [hibernateSessionFactory] ####

**Caused by:** 

javax.naming.NoInitialContextException: Need to specify class name in environment or system property, or as an applet parameter, or in an application resource file:  java.naming.factory.initial

**错误原因：**

在hibernate.cfg.xml中配置

<session-factory name="foo"></session-factory>

中添加了name属性

**解决方法：**

﻿去掉<session-factory name="foo"></session-factory>中的name属性

#### 3. 各种ClassNotFound错误/jboss错误 ####

Hibernate需要将相应的jar包放入Tomcat目录下的lib中；但是Struts2不能将jar包放入Tomcat中，不然会出现各种重复运行错误。

#### 4. No result defined for action action.localeAction and result input ####

可能原因：传入参数类型与action 中定义的数据类型不匹配

#### 5. Hibernate: Null value was assigned to a property of primitive type setter ####

hibernate int型数据无法读取 hiberante读int 整型数据出错 hiberante读数据库出错

**解决办法：**

javabean.hbm.xml

	<property name="provinceOrderMember" type="int">
	        <column name="provinceOrderMember">
	            <comment></comment>
	        </column>
	    </property>
其中，type="int" 改为type="java.lang.Integer"

javabean

private int provinceOrderMember; 改为private Intege provinceOrderMember

原因：数据库字段值为NULL,int 类型不能赋值为NULL，只能为0，但有些实际应用中，如学生分数，0表示0分，NULL，则表示暂无分数所以，要不设数据字段不为NULL，要不就是用Integer.

#### 5. HibernateException: javassit enhancement failed: XXX ####

**Caused by:**

相应的hibernate实体类中没有添加无参的构造函数

#### 6. java.lang.ClassCastException:  com.ch.hibernate.Student_$$_javassist_0 cannot be cast to javassist.util.proxy.Proxy####

**Caused by:**

lib下的类包有两个javassist包。

#### 7. Null value was assigned to a property of primitive type setter of XXX ####

**Caused by:**

【解释一】
hibernate int型数据无法读取 hiberante读int 整型数据出错 hiberante读数据库出错

解决办法：

javabean.hbm.xml

	<property name="provinceOrderMember" type="int">
	        <column name="provinceOrderMember">
	            <comment></comment>
	        </column>
	    </property>

type="int" 改为type="java.lang.Integer"

javabean

    private int provinceOrderMember; 改为private Intege provinceOrderMember

【解释二】
原因：数据库字段值为NULL,int 类型不能赋值为NULL，只能为0，但有些实际应用中，如学生分数，0表示0分，NULL，则表示暂无分数所以，要不设数据字段不为NULL，要不就是用Integer.

其中的类型为hibernate类型,在生成的类中，XXX字段为long(/int)类型，为基本类型，不能为NULL.

解决方法：

- 第一种：数据库字段不设置为空；
    
- 第二种：手动修改映射文件，printTime使用Java类型Long,即type="java.lang.Long"，Book类中的字段也要改为Long。同理，int为Integer.
    
-	第三种：在反向工程时使用Java类型，而不是hibernate类型。

#### 8. org.hibernate.LazyInitializationException: could not initialize proxy - no Session ####

**解决方法：**
     
（1）hibernate的配置文件中，lazy设置程false；

（2）Hibernate的DAO操作的Hql语句中，使用left join fetch 连接不同的表，例如：
     String hql = "from RouteDayDest rdd left join fetch rdd.dest d"
                + "left join fetch rdd.day dd where dd.id = ?" ;

#### 9. Positional parameter are considered deprecated; use named parameters or JPA-style positional parame ####

**原因：**

从告警提示信息中可以看出，它建议用命名参数或者JPA占位符两中种方法来代替老的占位符查询方法。

老的占位符查询代码片段：

	String hql = "select t from Blog t where t.site=? ";
	Query query = getSession().createQuery(hql);
	query.setParameter(0, "micmiu.com");

方法一：改成命名参数的方式：

	//命名参数的方式
	String hql2 = "select t from Blog t where t.site=:site ";
	Query query2 = getSession().createQuery(hql2);
	query2.setParameter("site", "micmiu.com");

方法二：改成JPA占位符的方式：

	//JPA占位符方式
	String hql3 = "select t from Blog t where t.site=?0 ";
	Query query3 = getSession().createQuery(hql3);
	query2.setParameter("0", "micmiu.com");

#### 10. Hibernate SQLGrammarException: could not extract ResultSet ####

**分析及解决方法：**

Change your query as below:

	String query = "from Code where Tags='" + tags+"'";

Otherwise as below:

	String hql = "from Code where Tags=:tags";Query query = session.createQuery(hql);
	query.setParameter("tags",tags);

The comparison in the where clause is with a literal and not another column. So it must be either quoted as in first case, or use a bind variable as in the second case.

#### 10. Hibernate数据库读写莫名失败错误####

数据库表中的column name不能使用系统保留的关键词，不然hibernate对数据库的读写操作会失败

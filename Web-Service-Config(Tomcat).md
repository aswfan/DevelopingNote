如何修改TOMCAT的默认主页为你自己项目的主页

博客分类： java
 
（最合适的）
最直接的办法是，删掉tomcat下原有Root文件夹，将自己的项目更名为Root。

我在$tomcat/webapps/下建了个myjsp目录作为我网站的默认目录，在myjsp中有一个a.jsp文件，该文件要作为我网站的默认主页。

修改配置文件：

首先，修改$tomcat/conf/server.xml文件。
在server.xml文件中，有一段如下：

    ……
    <engine name="Catalina" defaultHost="localhost">
    <host name="localhost" appBase="webapps"
    unpackWARs="true" autoDeploy="true"
    xmlValidation="false" xmlNamespaceAware="false">
    ……
<host>
</engine>
……

在<host></host>标签之间添加上：

    <Context path="" docBase="myjsp" debug="0" reloadable="true" />

path是说明虚拟目录的名字，如果你要只输入ip地址就显示主页，则该键值留为空；

docBase是虚拟目录的路径，它默认的是$tomcat/webapps/ROOT目录，现在我在webapps目录下建了一个myjsp目录，让该目录作为我的默认目录。

debug和reloadable一般都分别设置成0和true。

然后，修改$tomcat/conf/web.xml文件。
在web.xml文件中，有一段如下：

    <welcome-file-list>
    <welcome-file>index.html</welcome-file>
    <welcome-file>index.htm</welcome-file>
    <welcome-file>index.jsp</welcome-file>
    </welcome-file-list>

在<welcome-file-list>与<welcome-file>index.html</welcome-file>之间添加上：

    <welcome-file>a.jsp</welcome-file>

更改端口

    <Connector port="8080" maxThreads="150" minSpareThreads="25" maxSpareThreads="75" enableLookups="false" redirectPort="8443" acceptCount="100" debug="0" connectionTimeout="20000" disableUploadTimeout="true" /> 


将port "8080"改成你的端口

保存上述两个文件后重启tomcat，在浏览器地址栏内输入"http://localhost:8080/"，显示a.jsp页面的内容。

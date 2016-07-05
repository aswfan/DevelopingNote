#### 一、SVN配置（Ubuntu14.0环境shell） ####

	/*相关svn指令
	svn：命令行客户端
	svnadmin：用来创建、调整或修复版本库的工具
	svnserve：svn服务程序
	svndumpfilter：过滤svn版本库转储数据流的工具
	svnsync：svn数据同步同居，实现另外存一份相同的
	svnlook：用来查看版本中不同的修订版和事务
	*/

**1、安装svn**
    
	#sudo apt-get install subversion
    (注：初次使用install前需要先更新，即  #sudo apt-get update）

**2、创建版本库**
    
	#sudo mkdir /home/svn
    #sudo svnadmin create /home/svn/repos

**3、了解版本库**
     
	 #//进入版本库查看生成的相关文件
     #cd /home/svn/repos/
     #ls -al
	conf           //存放住配置文件和用户、权限配置
	db             //存放svn转储后的数据
	format     hooks     locks     README.txt

<p>

    #cd conf/
    #ls -al
	authz                  //设置用户权限
	passwd                 //存储用户及密码
	svnserve.conf          //住配置文件

**4、配置版本库**
  
4.1 修改主配置文件
      
	#sudo vi svnserve.conf

（操作说明）将以下参数去掉注释

	(1) [general]
	(2) anon-access = none     //匿名访问权限，默认read，none为不允许访问
	(3) auth-access = write     // 认证用户权限
	(4) passwd-db = passwd  //用户信息存放文件，默认在版本库/conf下面，也可以绝对路径指定文件位置
	(5) authz-db = authz

4.2 配置用户账户
    
	#sudo vi passwd
 
(操作说明）格式是“用户名=密码”，采用明文密码

	[Users]
	xiaoming = 123
	zhangsan = 123
	lisi = 123

  4.3 配置用户组
    
	#sudo vi authz
<p>

	[groups]
	manager = xiaoming
	core_dev = zhangsan,lisi
	[repos:/]           //以根目录起始的repos版本库manager组为读写权限
	@manager = rw
	[repos:/media]     //core_dev对repos版本库下media目录为读写权限
	@core_dev = rw

4.4 启动svn服务
      
	#sudo svnserve -d -r /home/svn
	//查看是否启动成功，可看的坚挺3690端口
    #sudo netstat -antp |grep svnserve
	//若想关闭服务，可使用 pkill svnserve

#### 二、Ubuntu 14.0 SVN使用报错实录： ####

##### 1. SVN: Cannot convert string from ‘UTF-8' to native encoding svn中存在中文命名的文件 #####

**Caused by:**

Linux本地环境默认语言不是’UTF-8‘

**Solution:**

reset the locale

	$export LC_CTYPE=en_US.UTF-8
	$export LANG=en_US.UTF-8

After that, could check the locale settings by:
    
	#locale

##### 2. SVN: Unable to connect to a repository at URL: 'http://XXXX'can't connect to host: 'XXXX': 由于目标计算机积极拒绝，无法连接 #####

**Caused by:** 

SVN服务（svnserver）未开启

**Solution:**

 开启svnserver服务

	//在任务管理器中查看svnserver是否开启
	$sudo ps -aux|grep svnserver
	//打开指定目录下的svn服务
	$sudo svnserve -d -r /xxx/...

##### 3. SVN: Repository UUID doesn't match expected: 'XXXX' UUID: 'XXXX' #####

 **Analysis:** 

当在服务器创建一个svn版本库的时候，会自动生成一个uuid文件，该文件位于版本库的db目录下，用来唯一标识一个版本库。当在客户端checkout的版本库中执行svn命令时，就会将本地的保存的uuid与服务器端版本库中的uuid进行核对，如果不一致就会报该错误。

**Caused by:** 

服务器端svnserver服务开启错误的版本库，未在目标版本库的路径下开启svnserver服务

**Solution:**

关闭错误版本库的服务；在目标版本库的路径下开启svnserver服务
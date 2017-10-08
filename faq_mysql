#### 一、MySQL连接问题： ####

windows mysql提示：1045 access denied for user 'root'@'localhost' using password yes ：

**Caused by：** 

密码错误

**Resolution：**
     
1、任务管理器：kill后台mysqld的所有进程
     
2、管理员权限打开cmd：

	>mysqld -n --skip-grant-tables //以跳过密码验证的方式新建mysqld进程
 
![](https://raw.githubusercontent.com/aswfan/MarkdownPhoto/master/myPhotos/mysql-1.png)

3、再以管理员权限打开一个新的cmd：
               
	>mysql  //进入mysql控制台
	mysql>flush privileges
	mysql>...               //修改密码等操作
	//修改密码需要用到PASSWORD（）方法才行

![](https://raw.githubusercontent.com/aswfan/MarkdownPhoto/master/myPhotos/mysql-2.png)

4、退出mysql：
    
	>exit

#### 二、MySQL完全卸载： ####

- 控制面板——》所有控制面板项——》程序和功能，卸载mysql server!

![](https://raw.githubusercontent.com/aswfan/MarkdownPhoto/master/myPhotos/mysql-3.jpg)

2. 然后删除mysql文件夹下的my.ini文件及所有文件

![](https://raw.githubusercontent.com/aswfan/MarkdownPhoto/master/myPhotos/mysql-4.jpg)

3. 运行“regedit”文件，如图，打开注册表

![](https://raw.githubusercontent.com/aswfan/MarkdownPhoto/master/myPhotos/mysql-5.jpg)

4.  删除
 (1)HKEY_LOCAL_MACHINE\SYSTEM\ControlSet001\Services\Eventlog\Application\MySQL文件夹
 (2)HKEY_LOCAL_MACHINE\SYSTEM\ControlSet002\Services\Eventlog\Application\MySQL文件
 (3)HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\Eventlog\Application\MySQL的文件夹。
如果没有可以不用删除了。

5.  删除C盘下的“C:\ProgramData\MySQL ”所有文件，如果删除不了则用360粉碎掉即可，该programData文件是隐藏的默认，设置显示后即可见，或者直接复制上边的地址到地址栏回车即可进入！删除后重启电脑，重装MYsql数据库应该就成功了。

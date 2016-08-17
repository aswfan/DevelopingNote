#### - str与int的转换 ####

		string = str(int)
		int = int(str)

#### - python通过MySQLdb连接数据库时 ####


        conn = MySQLdb.connect(
                               host = 'localhost ',
                               port = 3306,
                               user = 'root',
                               passwd = '123456',
                               db = 'travel',
                               charset = 'utf8'
                               )
        cur = conn.cursor()
        sql = 'update route set love=%s where name=%s'
        cur.execute(sql , (int(data['love']), data[ 'name'].encode('utf                           -8')))
        cur.close()
        conn.commit()
        conn.close()

若有返回值，则用：
	
	result = cur.fetchall()

其中，需要注意：

数据库中表的属性不能使用保留的关键字（如like等），不然在python中会报错！

#### - python使用Beautifulsoup框架分析网页DOM结构 ####

	soup = BeautifulSoup(html_cont, 'html.parser' , from_encoding='utf -8')

当查找与标签类型或css有关时：
	
	soup.find_all( 'li', class_= 'list_item' )

当查找对象与中文字符有关时：

	str = ''
	url_node = node.find( 'a', href=re.compile(r"/youji /\d+"))
	source = url_node .getText()
	temp = source #.decode('utf8')
	xx=u"[\u4e00-\u9fa5]+"
	results = re.compile(xx).findall(temp)
	for it in results:
	     str = str + it
	data['name'] = str

当查找的是某标签的父节点时：

	soup.find(class_= 'unit').find_parent('div ')

当查找对象与标签内内容有关时：

	soup.find(string=re.compile( r'\d+'))

当查找对象与下一个姊妹节点有关时：

	soup .next_sibling

获取节点中的信息：

- 获取节点中的内容：

		node.getText()

- 获取节点中的属性：

		node['href ']

#### - 正则表达式的使用 ####

筛选（中文）字符:

     temp = '<dt class="sub_tit">人　均</dt>啊设计'
     soup = BeautifulSoup(temp, 'html.parser', from_encoding= 'utf -8')
     temp = temp.decode('utf8')
     xx=u"[\u4e00-\u9fa5]+"
     pattern = re.compile(xx)
     # result =  soup.find(class_='sub_tit', string=re.compile(u'类　          型')).next_sibling
     results = pattern.findall(temp)
     str = ''
     for result in results:
         str = str + result
     print str

查找符合匹配规则的字符：

     #coding:utf-8
     import re
     source = "s2f程序员杂志一2d3程序员杂志二2d3程序员杂志三2d3程序员杂志四2d3"
     temp = source.decode('utf8')
     #unicode中所有的中文字符编码范围
     xx=u"[\u4e00-\u9fa5]+"
     pattern = re.compile(xx)
     results =  pattern.findall(temp)
     for result in results :
         print result

替换相对应的字符：

     str.replace(u'出发','')

#### - 关于数值类型 ####

查询数据库的返回值是tuple类型（即（），不能修改的list）
[]为数组
{}为set

set具有contains功能

#### - python下载网页 ####

     def download(self, url):
        if url is None:
            return
        response = urllib2.urlopen(url)
        if response.getcode() != 200:
            return
#         print response.read()
        return response.read()

#### - python函数初始化 ####


【例一】
    
	 class HtmlOutputer(object):
    """docstring for HtmlOutputer"""
         def __init__(self):
              self.datas = []

【例二】

     from crawl4gonglue import url_manager, html_downloader, html_outputer,      html_parser
     class spiderMain(object):

         def __init__(self):
              self.urls = url_manager.UrlManager()
              self.downloader = html_downloader.HtmlDownloader()
              self.parser = html_parser .HtmlParser()
              self.outputer = html_outputer.HtmlOutputer()

【Main函数】

     if __name__ == '__main__':

#### - python向mysql中写入数据时，中文字符乱码的解决： ####

	conn = MySQLdb.connect(host='localhost', user='root',passwd='123', db='account', charset='utf8')  
	# OK，如果没有charset='utf8'，插入为乱码

#### - python Windows环境下交互cp65001异常 ####

Python安装后进入命令行交互模式，输入任何代码都报
	
	unknown encoding: cp65001异常

需要将编码(UTF-8)修改为 简体中文(GBK)
在CMD窗口执行　
	
	chcp 936
#### - Set自定义类Contain方法的实现 ####

http://blog.csdn.net/renfufei/article/details/14163329

若要在Set自定义类中实现Contain方法，需要在自定义类中override根类Object中的Equals和HashCode两个方法（具体实现代码参考Eclipse自动生成的代码“source -> generate equals and hashcode

#### - Collection的sort方法的实现 ####

Collection.sort(待排序的list, Comparator的实现类)
Comparator实现类是sort方法的关键，参考例子：

	Collections.sort(myList, new sortByDistance());
	
	class sortByDistance implements Comparator<DistanceBean>{
	       @Override
	        public int compare(DistanceBean d1, DistanceBean d2) {
	             if(d1 .getDistance()-d2.getDistance()>0)
	                    return -1;
	              else if (d1 .getDistance()-d2.getDistance()<0)
	                    return 1;
	              else
	                    return 0;
	            }


#### - Iterator(迭代器）的一般用法 ####

迭代器是一种设计模式，它是一个对象。它可以遍历并选择序列中的对象，而开发人员不需要了解该序列的底层结构。迭代器通常被称为“轻量型”对象，因为创建他们的代价小。

Java中的interator()功能较简单，只能单向移动。而为List设计的ListIterator具有更多功能，它可以从两个方向遍历List，也可以从List中插入或删除元素。

注意：iterator()方法是java.lang.Iterable接口，被Collection类继承。

迭代器应用：
 
	list l = new ArrayList();
	l.add("aa");
	l.add("bb");
	l.add("cc");
	for (Iterator iter = l.iterator(); iter.hasNext();) {
	String str = (String)iter.next();
	System.out.println(str);
	}
	/*迭代器用于while循环
	Iterator iter = l.iterator();
	while(iter.hasNext()){
	String str = (String) iter.next();
	System.out.println(str);
	}
	*/



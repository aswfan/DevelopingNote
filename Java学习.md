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


#### - Activity间传List ####

传List：

      Bundle bundle=new Bundle();
      bundle.putParcelableArrayList("list", (ArrayList)list1);
      intent.putExtras(bundle);
      startActivity(intent);

收List：

	Bundle bundle=this.getIntent().getExtras();
	ArrayList list2 = bundle.getParcelableArrayList("list");


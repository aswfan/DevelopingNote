#### - Error: [android] No resource found that matches the given name 'Theme.AppCompat.Light', etc ####

**Description:**

 When importing exist android project into current working space, it shows the above errors in the console.

**Caused by:**

Missing the supporting lib: appcompat_v7

**Solution:**
     
Create a new android project in the current working space and then Eclipse will automatically create the lib appcompat_v7. To fix the error, you have to create the reference for the imported project in the project's "properties-android-lib"

![](https://raw.githubusercontent.com/aswfan/MarkdownPhoto/master/myPhotos/android%20note-1.png)

#### - Error: R cannot be resolved to a variable ####

**Description:**
     
All of a sudden, the project cannot run and shows the above error in the console

**Caused by:**
     
The building file(project.properties/AndroidManifest.xml) of the project has some errors

**Solution:**
     
Step 1: Fix the errors in the building files
     

Step 2: Select the project and click "build automatically" in the 'project' of the menu
     
Step 3: Select the project and click "clean" in the 'project' of the menu
     
Finally, a R.java will be created automatically in the sub-directory 'gen'

#### - ERROR:  Genymotion总报错：Fatal signal 11 (SIGSEGV) ####

在Windows环境使用Genymotion的时候，总报如下的错误：

	[plain] view plain copy
	<span style="font-family:SimSun;font-size:14px;">06-01 17:50:19.145: A/libc(19868): Fatal signal 11 (SIGSEGV) at 0xdeadd00d (code=1), thread 19890
	06-01 17:59:58.969: A/libc(20434): Fatal signal 11 (SIGSEGV) at 0x890875cb (code=1), thread 20434</span>

查找网贴（http://www.programlife.net/debugger-magic-number.html）得知，SIGSEGV是Signal Segmentation Violation。

这属于很底层的问题了，应该是JAVA虚拟机以下，更准确地说，是C语言（so库文件或so的运行环境）代码导致的。

	0xDEADD00D ("dead dude") is used by Android in the Dalvik virtual machine to indicate a VM abort.

在网贴（http://stackoverflow.com/questions/20640647/fatal-signal-11-sigsegv-code-2-on-genymotion-emulator-not-using-ndk）中，

给出了非常具备参考价值的建议：

![](https://raw.githubusercontent.com/aswfan/MarkdownPhoto/master/myPhotos/android%20note-2.png)

	我用的也是4.3 X86的镜像（Image），我试了“vmSafeMode=ture”，没用。问题依然存在。
	我又试了将镜像切换到4.2.2，问题解决了。太好了！太帅了！太棒了！
	
	补充：
	切换镜像后，如果运行APK时出现错误：INSTALL_FAILED_CPU_ABI_INCOMPATIBLE，
	则需要下载Genymotion-ARM-Translation，并将其拖入到虚拟机镜像内，方可。

#### - AndroidManifest.xml文件中引用Theme.NoTitleBar-Android报找不到错误 ####

**原因：**

因为这个主题不在你的工程中，而是系统自带的，所以要这样子：android:theme="@android:style/Theme.NoTitleBar"
在java里面用 requestWindowFeature(Window.FEATURE_NO_TITLE); 也不起作用

#### - Java.util.ConcurrentModificationException ####

**描述：**

	 An ConcurrentModificationException is thrown when a Collection is modified and an existing iterator on the Collection is used to modify the Collection as well.

ConcurrentModificationException 抛出的条件   大意是: 一个迭代器在迭代集合的时候   集合被修改了


#### - 含CheckBox等的ListView中ItemClickListener失效的问题 ####

**描述：**

当ListView的Item是CheckBox、Button等控件时，因为CheckBox和Button的控件click事件优先级高于ListView的ItemClickListener事件，所以在给这些CheckBox或Button设置了click事件后，会导致ListView的ItemClickListener失效

**解决方法：**

不设置CheckBox、Button等子控件的click事件，通过设置ListView的ItemClickListener事件，来间接的完成CheckBox或Button等的click事件

例：

ListView的Item界面（xml）：

	<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
	
	    android:layout_width="match_parent"
	
	    android:layout_height="match_parent"
	
	    android:orientation="vertical"
	    android:descendantFocusability="blocksDescendants"
	    >
	    <CheckBox
	
	         android:id="@+id/checkBox"
	
	         android:layout_width="fill_parent"
	
	         android:layout_height="wrap_content"
	
	         android:text=""
	         android:textColor="#000000"
	         android:layout_marginBottom="5dp"
	         android:lines="1"
	         android:clickable="false"
	         android:focusable="false"
	         />
	</LinearLayout>


ListView的ItemClick事件：

	list.setOnItemClickListener(new OnItemClickListener(){
	
	     @Override
	     public void onItemClick(AdapterView<?> paramAdapterView, View paramView , int position, long paramLong ) {
	     // TODO Auto-generated method stub
	          Map<Integer, Boolean> selected = adapter.getSelected();
	          selected.put(position , !selected.get( position));
	          adapter.setSelected(selected );
	          adapter.notifyDataSetChanged();
	          String del = "," + String.valueOf(maps.get( position).get("id" ));
	          likes += del ;
	          dislikes.replace(del , "," );
	      }
	});
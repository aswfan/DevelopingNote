- 非法的表达式开头

当编译器遇到一条不合法的语句时会报“非法的表达式开头”这种错误。考虑下面的例子：

	public class Test {
	    public static void main(String[] args) {
	        my_method();


	    public static void my_method() {
	        System.out.println("Hello, world!");
	    }
	}
<p>

	5 errors found:
	File: Test.java  [line: 6]
	Error: Test.java:6: illegal start of expression
	File: Test.java  [line: 6]
	Error: Test.java:6: illegal start of expression
	File: Test.java  [line: 6]
	Error: Test.java:6: ';' expected
	File: Test.java  [line: 6]
	Error: Test.java:6: ';' expected
	File: Test.java  [line: 9]
	Error: Test.java:9: reached end of file while parsing

这里，缺少了一个关闭main方法大括号。由于main方法没有被关闭，编译器把调用my_method方法之后的代码也当作main方法的一部分。然而，后面的代码是public static void my_method() {，很显然，这在一个方法内不合法。

“非法的表达式开头”这种错误不如我们上面提到的“××缺失”这种信息有帮助。对于这种错误（以及很多其他一些错误），非常有必要检查一下出错代码前面的那几行。对于上面那个例子，我们只需要在编译器报错的那行前面加上大括号关闭main方法就可以了。重新编译，所有的错误都解决了。

	public class Test {
	    public static void main(String[] args) {
	        my_method();
	    }
	
	    public static void my_method() {
	        System.out.println("Hello, world!");
	    }
	}
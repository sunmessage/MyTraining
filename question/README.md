# 概述
这里放一些对自己而言比较典型的，或者有价值的一些面试题。

# 试题
下面选项中,哪些是interface中合法方法定义?()
- [x] public void main(String [] args);
- private int getSum();
- [x] boolean setFlag(Boolean [] test);
- [x] public float get(int x);
<br>
解析：<br>
`interface中的方法默认为public abstract 的 ，变量默认为public static final` <br>
java程序的入口必须是static类型的，接口中不允许有static类型的方法。A项没有static修饰符，可以作为普通的方法。而且接口中的方法必须是public的。想想借口就是为了让别人实现的，相当于标准，标准不允许别人使用是不合理的，所以接口中的方法必须是public。C项中，接口中的方法默认是public的。D项属于正常的方法。所以答案是：ACD

---

下列Java代码中的变量a、b、c分别在内存的____存储区存放。

``` stylus
class A {
    private String a = “aa”;
    public boolean methodB() {
        String b = “bb”;
        final String c = “cc”;
    }
}
```
- 堆区、堆区、堆区
- 堆区、栈区、堆区
- [x] 堆区、栈区、栈区
- 堆区、堆区、栈区
- 静态区、栈区、堆区
- 静态区、栈区、栈区

解析：<br>
a是类中的成员变量，存放在堆区；b、c都是方法中的局部变量，存放在栈区。<br>
参考《Head First Java》p236，对象的生存空间（堆，heap）和方法调用及变量的生存空间（栈，stack），以及基本数据类型变量也放在栈上。实例变量存在于所属的对象中，局部变量和方法参数都被声明在方法中。<br>
要理解的是，**对象本身只会存在堆上**，即如果方法的局部变量是个对对象的引用，也只有变量本身放在栈上，所引用的对象不会放到栈里。

---

ArrayList list = new ArrayList(20);中的list扩充几次
- [x] 0
- 1
- 2
- 3
解析：<br>
1. ArrayList底层是数组，默认构造函数的创建初始容量是10，动态扩容的公式是：新容量=就容量/2 + 旧容量 + 1。比如初始容量为4，每次扩容后：4 -> 7 -> 11 -> 17 -> 26；
2. 这里调用的是一个可以指定初始容量的构造函数，直接初始化大小为20，所以没有扩容。

---

``` stylus
byte b1=1,b2=2,b3,b6; 
final byte b4=4,b5=6; 
b6=b4+b5; 
b3=(b1+b2); 
System.out.println(b3+b6);
```
关于上面代码片段叙述正确的是（）
- 输出结果：13
- 语句：b6=b4+b5编译出错
- [x] 语句：b3=b1+b2编译出错
- 运行期抛出异常
解析：<br>
被final修饰的变量是常量，这里的b6=b4+b5可以看成是b6=10，在编译时就已经变为b6=10了。<br>
而b1和b2是byte类型，java中进行计算时候将他们提升为int类型，再进行计算，b1+b2计算后已经是int类型，赋值给b3，b3是byte类型，类型不匹配，编译不会通过，需要进行强制转换。<br>
Java中没有final修饰的的byte，short，char进行计算时都会提升为int类型。<br>

> ①所有的byte,short,char型的值将被提升为int型； ②如果有一个操作数是long型，计算结果是long型；
> ③如果有一个操作数是float型，计算结果是float型； ④如果有一个操作数是double型，计算结果是double型；
> 而声明为final的变量会被JVM优化，第6行相当于 b6 = 10

---

``` stylus
1.public class Enclosingone
2.{
3.    public class InsideOne {}
4.
5.}
6.public class inertest
7.{
8.    public static void main(string[]args)
9.    {
10.        EnclosingOne eo = new EnclosingOne();
11.        //insert code here
12.    }
13.
14.}
```
Which statement at line 11 constructs an instance of the inner class?
- InsideOne ei=eo.new InsideOne();
- eo.InsideOne ei=eo.new InsideOne();
- InsideOne ei=EnclosingOne.new InsideOne();
- [x] EnclosingOne.InsideOne ei=eo.new InsideOne();

解析：<br>
内部类其实和类的属性没什么区别，只是在声明的时候必须是Outer.Inner a，就像int a 一样，至于静态内部类和非静态内部类new的时候有点区别，Outer.Inner a=new Outer().new Inner()（非静态，先有Outer对象才能有属性） Outer.Inner a=new Outer.Inner()要把Outer.Inner看成一部分，就像类变量一样。

---

以下集合对象中哪几个是线程安全的（）
- LinkedList
- ArrayList
- [x] Vector
- [x] Hashtable

解析：<br>
集合中线程安全的类有：vector，stack，hashtable，enumeration，除此之外均是非线程安全的类与接口。

---

``` stylus
public class NameList
{
    private List names = new ArrayList();
    public synchronized void add(String name)
    {
        names.add(name);
    }
    public synchronized void printAll()     {
        for (int i = 0; i < names.size(); i++)
        {
            System.out.print(names.get(i) + ””);
        }
    }
 
    public static void main(String[]args)
    {
        final NameList sl = new NameList();
        for (int i = 0; i < 2; i++)
        {
            new Thread()
            {
                public void run()
                {
                    sl.add(“A”);
                    sl.add(“B”);
                    sl.add(“C”);
                    sl.printAll();
                }
            } .start();
        }
    }
}
```
Which two statements are true if this class is compiled and run?<br>
- An exception may be thrown at runtime.
- The code may run with no output, without exiting.
- The code may run with no output, exiting normally(正常地).
- The code may rum with output “A B A B C C “, then exit.
- [x] The code may rum with output “A B C A B C A B C “, then exit.
- The code may ruin with output “A A A B C A B C C “, then exit.
- [x] The code may ruin with output “A B C A A B C A B C “, then exit.
解析：<br>
有两条进程在同时进行，线程内的是顺序进行，线程之间的顺序不定，可能是交叉执行，也有可能是先执行了一个线程后执行了另一个线程。<br>
所以第一次输出，最少可能输出3个即直接运行完整个第一个线程，最多可能输出6个就是两个线程交叉运行直到双方线程的addA、addB、addC都执行完成然后执行其中一方的print。<br>
所以分析一下几个答案：
- A B A B C C （第一次print一定有C，所以第一次输出ABABC，第二次只输出了C，不可能，错）
- A B C A B C A B C （第一次print输出ABC，然后执行第二线程，再输出ABCABC，最终ABCABCABC，正确）
- A A A B C A B C C （AAA不可能出现，错）
- A B C A A B C A B C （第一次输出ABCA，第二次输出ABCABC，最终输出ABCAABCABC，正确）
第二次输出的内容前部分一定是第一次输出的内容，如G选项中第二次输出A B C A B C，前半部分正是第一次输出的ABCA。

---


















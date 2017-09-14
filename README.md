JVM内存：
1.程序计数器是一个比较小的内存区域，用于指示当前线程所执行的字节码执行到了第几行，是线程隔离的。
2.虚拟机栈描述的是Java方法执行的内存模型，用于存储局部变量；操作数栈，动态链接，方法出口等信息，是线程隔离的。
3.方法区用于存储JVM加载类的信息，常量，静态变量、以及编译器编译后的代码等数据，是线程共享的。
4.原则上讲，所以的对象都在堆区上分配内存，是线程之间共享的。

JDBC statement：
1.JDBC提供了Statment,PrepareStatement 和 CallableStatement 三种方式来执行查询语句，其中Statement用于通用查询，CallableStatement 用于存储过程，PrepareStatement用于执行参数化查询。
2.PrepareStatement,数据库可以使用已经编译过及定义好的执行计划，速度比Statment快。
3.PrepareStatement中，？叫做占位符，一个占位符一个值,可以防止SQL注入。

SPRING 的事务传播性
1.PROPAGATION_SUPPORTS ：支持当前事务，如果当前没有事务，就以非事务方式执行。
2.PROPAGATION_REQUIRED: 支持当前事务，如果当前没有事务就新建一个事务。
3.PROPAGATION_REQUIRES_NEW ： 新建事务，如果当前存在事务，把当前失事务挂起。
4.PROPAGATION_NESTED ：支持当前事务，新增Savepoint ,与当前事务同步提交或回滚。
5.PROPAGATION_MANDATORY: 支持当前事务，如果没有当前事务，就抛出异常。
6.PROPAGATION_NEVER : 以非事务方式执行，如果当前存在事务，则抛出异常。


Servlet和CGI的区别
1.Servlet被服务器实例化后，容器运行其init方法，请求到达时运行其service方法，service方法自动派遣运行与请求对应的doXXX方法（doGet，doPost）等，当服务器决定将实例销毁的时候调用其destroy方法。
servlet处于服务器进程中，它通过多线程方式运行其service方法，一个实例可以服务于多个请求，并且其实例一般不会销毁，而CGI对每个请求都产生新的进程，服务完成后就销毁，所以效率上低于servlet。
概括来讲，Servlet可以完成和CGI相同的功能。
2.CGI应用开发比较困难，因为它要求程序员有处理参数传递的知识，这不是一种通用的技能。CGI不可移植，为某一特定平台编写的CGI应用只能运行于这一环境中。每一个CGI应用存在于一个由客户端请求激活的进程中，并且在请求被服务后被卸载。这种模式将引起很高的内存、CPU开销，而且在同一进程中不能服务多个客户。
3.Servlet提供了Java应用程序的所有优势——可移植、稳健、易开发。使用Servlet Tag技术，Servlet能够生成嵌于静态HTML页面中的动态内容。
4.Servlet对CGI的最主要优势在于一个Servlet被客户端发送的第一个请求激活，然后它将继续运行于后台，等待以后的请求。每个请求将生成一个新的线程，而不是一个完整的进程。多个客户能够在同一个进程中同时得到服务。一般来说，Servlet进程只是在Web Server卸载时被卸载。
5.Servlet在性能、编写难度、可移植性等方面比CGI有明显优势。在WebSphere Application Server中提供了功能强大的Servlet API，它们比JSDK拥有更多的功能和更优的性能，为Servlet的编程提供了很好的支持。随着WAS的日益推广和Java技术的普及，可以预见，Servlet技术将取代CGI，成为对Web Server功能扩充的标准技术。

servlet service 的描述
1.doGet/doPost 是在javax.servlet.http.HttpServlet中实现的。
2.servlet()是在javax.servlet.Servlet接口中定义的。
3.servlet在多线程下其本身并不是线程安全的。
如果在类中定义成员变量，而在service中根据不同的线程对该成员变量进行更改，那么在并发的时候就会引起错误。最好是在方法中，定义局部变量，而不是类变量或者对象的成员变量。由于方法中的局部变量是在栈中，彼此各自都拥有独立的运行空间而不会互相干扰，因此才做到线程安全。

forward 和 redirect 
1.forward 请求转发 服务器获取跳转页面内容传给用户，地址栏不变
2.redirect  重定向 服务器向用户发送转向的地址，地址变


关于sleep()和 wait()
Java中的多线程是一种抢占式的机制，而不是分时机制。抢占式的机制是有多个线程处于可运行状态，但是只有一个线程在运行。 
共同点 ： 
1. 他们都是在多线程的环境下，都可以在程序的调用处阻塞指定的毫秒数，并返回。 
2. wait()和sleep()都可以通过interrupt()方法 打断线程的暂停状态 ，从而使线程立刻抛出InterruptedException。 
如果线程A希望立即结束线程B，则可以对线程B对应的Thread实例调用interrupt方法。如果此刻线程B正在wait/sleep/join，则线程B会立刻抛出InterruptedException，在catch() {} 中直接return即可安全地结束线程。 
需要注意的是，InterruptedException是线程自己从内部抛出的，并不是interrupt()方法抛出的。对某一线程调用 interrupt()时，如果该线程正在执行普通的代码，那么该线程根本就不会抛出InterruptedException。但是，一旦该线程进入到 wait()/sleep()/join()后，就会立刻抛出InterruptedException 。 
不同点 ：  
1.每个对象都有一个锁来控制同步访问。Synchronized关键字可以和对象的锁交互，来实现线程的同步。 
sleep方法没有释放锁，而wait方法释放了锁，使得其他线程可以使用同步控制块或者方法。 
2.wait，notify和notifyAll只能在同步控制方法或者同步控制块里面使用，而sleep可以在任何地方使用 
3.sleep必须捕获异常，而wait，notify和notifyAll不需要捕获异常 
4.sleep是线程类（Thread）的方法，导致此线程暂停执行指定时间，给执行机会给其他线程，但是监控状态依然保持，到时后会自动恢复。调用sleep不会释放对象锁。
5.wait是Object类的方法，对此对象调用wait方法导致本线程放弃对象锁，进入等待此对象的等待锁定池，只有针对此对象发出notify方法（或notifyAll）后本线程才进入对象锁定池准备获得对象锁进入运行状态。

异常

1.运行时异常： 都是RuntimeException类及其子类异常，如NullPointerException(空指针异常)、IndexOutOfBoundsException(下标越界异常)等，这些异常是不检查异常，程序中可以选择捕获处理，也可以不处理。这些异常一般是由程序逻辑错误引起的，程序应该从逻辑角度尽可能避免这类异常的发生。
运行时异常的特点是Java编译器不会检查它，也就是说，当程序中可能出现这类异常，即使没有用try-catch语句捕获它，也没有用throws子句声明抛出它，也会编译通过。 
2.非运行时异常 （编译异常）： 是RuntimeException以外的异常，类型上都属于Exception类及其子类。从程序语法角度讲是必须进行处理的异常，如果不处理，程序就不能编译通过。如IOException、SQLException等以及用户自定义的Exception异常，一般情况下不自定义检查异常。


动态include 和静态include

1.动态 INCLUDE 用 jsp:include 动作实现 <jsp:include page="included.jsp" flush="true" /> 它总是会检查所含文件中的变化 , 适合用于包含动态页面 , 并且可以带参数。各个文件分别先编译，然后组合成一个文件。
2.静态 INCLUDE 用 include 伪码实现 , 定不会检查所含文件的变化 , 适用于包含静态页面 <%@ include file="included.htm" %> 。先将文件的代码被原封不动地加入到了主页面从而合成一个文件，然后再进行翻译，此时不允许有相同的变量。 
3.以下是对 include 两种用法的区别 ， 主要有两个方面的不同 ;
    一 : 执行时间上 :
    <%@ include file="relativeURI"%> 是在翻译阶段执行
    <jsp:include page="relativeURI" flush="true" /> 在请求处理阶段执行 .
    二 : 引入内容的不同 :
    <%@ include file="relativeURI"%>  
    引入静态文本 (html,jsp), 在 JSP 页面被转化成 servlet 之前和它融和到一起 .
    <jsp:include page="relativeURI" flush="true" /> 引入执行页面或 servlet 所生成的应答文本 .
    
JVM内存配置参数

-Xmx：最大堆大小
-Xms：初始堆大小
-Xmn:年轻代大小
-XXSurvivorRatio：年轻代中Eden区与Survivor区的大小比值
年轻代5120m， Eden：Survivor=3，Survivor区大小=1024m（Survivor区有两个，即将年轻代分为5份，每个Survivor区占一份），总大小为2048m。
-Xms初始堆大小即最小内存值为10240m




何为类方法？

是指类中用static修饰的方法，非static为实例方法。
因此在类方法中不能用this来调用本类方法，通过类名直接调用。
实例方法必须创建对象（new） 。


容器集合类之间的关系

![image](https://github.com/a408228147/photo/blob/master/photo/collection.jpg)


& 和&&  | 和 ||的区别

&和&&都是逻辑运算符号，&&又叫短路运算符  &也是位运算符
区别如下
&　不管前面的条件是否正确，后面都执行
&&　前面条件正确时，才执行后面，不正确时，就不执行，就效率而言，这个更好
按位与 & 运算规则 转化为二进制 如果两个数都都为真(或为1)，其结果为真，如果两位数中有一位为假(或为0）者结果为假。
| || 和上同理。


看一段代码
       int count = 0;
        for (int i = 0;i<5; i++)
        {
            count = count++;
        }
       System.out.println(count);//count == 0
   
   原因是 后自增的优先级高于 = ， 但是后自增返回的是自增前的结果，因此就算count++自增了1，但是返回的是0，又重新赋值给了count。
   所以
   int count = 0;
   int sum = 0；
       for (int i = 0;i<5; i++)
        {
            sum = count++;
        }
       System.out.println(count);//count == 5
       System.out.println(sum);//count == 4
       
       
       静态static 不能修饰外部类，但是可以修饰内部类。
     
     正则表达式中的贪婪 与非贪婪模式 
1.什么是正则表达式的贪婪与非贪婪匹配
　　如：String str="abcaxc";
　　　　Patter p="ab*c";
　　贪婪匹配:正则表达式一般趋向于最大长度匹配，也就是所谓的贪婪匹配。如上面使用模式p匹配字符串str，结果就是匹配到：abcaxc(ab*c)。
　　非贪婪匹配：就是匹配到结果就好，就少的匹配字符。如上面使用模式p匹配字符串str，结果就是匹配到：abc(ab*c)。
2.编程中如何区分两种模式
　　默认是贪婪模式；在量词后面直接加上一个问号？就是非贪婪模式。
　　量词：{m,n}：m到n个
　　　　　*：任意多个
　　　　　+：一个到多个
　　　　　？：0或一个
     例：“.*?(?=\\() ”  北京（海定区）（朝阳）
     匹配到的是 北京
     (?=\\() → (? = assert) 顺序环视
     \\为转义字符 
     .*?  匹配到的字符后面必须紧跟有assert中申明的值，也就是左括号 ，但是匹配到的串是不包含assert中申明的内容
     
     
     
     
     

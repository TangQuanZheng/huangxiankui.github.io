#android笔试面试—要点总结




  本文整理自[飞奔的蜗牛](https://huangxiankui.github.io/blog/index.html)——转载请加以说明

- **android笔试面试一览**
- **android笔试——java常见习题**
- **android笔试——基础知识点**
- **android笔试——要点**
-  **算法题**



##android笔试面试一览
> [飞奔的蜗牛](https://huangxiankui.github.io/blog/index.html)目前主要从事于android开发，不久前才经历过校园招聘洗礼，故对校园招聘中关于android开发笔试、面试知识要点做整理（知识点挖的比较浅，只是应付笔试面试），对于需要补充添加的地方，欢迎各位邮件（1902136697@qq.com)或直接评论，我会添加并完善，不足之处，欢迎指正。

##android笔试——java常见习题


###1.== 与equals判断
equals用于比较俩个对象内部的内容是否相等
 = =判断是否指同一个对象
 如何熟练记住：
```java
 String a=”1234“；
 String b=”1234“；
 String c=new String（”1234“）；
 a==b  true  都是string对象
 a==c false  不是指向一个对象
```
###2.抽象类与接口的差别
1、抽象类可以有构造方法，接口没有构造方法
2、抽象类可以有普通成员变量，接口中没有普通成员变量
3、抽象类中可以有非抽象的普通方法，接口中所有方法都是抽象的
4、抽象类中抽象方法的访问类型可以是pubic、protected，接口中抽象方法只能是public，默认为public abstract
5、抽象类、接口中都可以包含静态成员，抽象类中静态成员变量的访问类型可以任意，但接口中的变量只能是public static final
6、一个类可以实现多个接口，但只能继承一个抽象类

###3.单例模式
保证系统中一个类只有一个实例，而且该实例易于外界访问
```java
public class SingleTon{
private SingleTon(){};
private static SingleTon singleton=null;
public static SingleTon getInstance()
{
if(sigleton==null)
synchronized(Singleton.class)
{
if(singleton==null)
{
singleton=new Singleton();
}
}
}
return singleton();
}
```

###4.观察者模式
对象间一对多的依赖关系，当一个对象的状态发生改变时，所有依赖于他的对象都得到通知并被自动更新。

###5.private、protected、public和default的区别
![这里写图片描述](http://img.blog.csdn.net/20151022171526313)

###6.Java并发问题
sleep()和 wait()有什么区别?
        答：sleep是线程类（Thread）的方法，导致此线程暂停执行指定时间，给执行机会给其他线程，但是监控状态依然保持，到时后会自动恢复。调用sleep不会释放对象锁。
wait是Object类的方法，对此对象调用wait方法导致本线程放弃对象锁，进入等待此对象的等待锁定池，只有针对此对象发出notify方法（或notifyAll）后本线程才进入对象锁定池准备获得对象锁进入运行状态。
[java并发详解](http://blog.csdn.net/geolo/article/details/8670900)

##android笔试——基础知识点
###1、选择题

1.java.io包中定义了多个流类型来实现输入和输出功能，可以从不同的角度对其进行分   类,按功能分为：(),如果为读取的内容进行处理后再输出，需要使用下列哪种流？()

     A、输入流和输出流  B、字节流和字符流  C、节点流和处理流 

     D、File stream  E、Pipe stream  F、Random stream  G、Filter stream
  2.下列代码的执行结果是：（）        public class Test3{        public static void main(String args[]){        System.out.print(100%3);        System.out.print(",");        System.out.println(100%3.0);        }        }        A、1,1        B、1,1.0          C、1.0,1        D、1.0,1.0

   3.在继承中，关于构造方法的说明，下列说法错误的是（）

A、子类无条件的继承父类的无参构造方法，
B、子类可以引用父类中的有参构造方法，使用super关键字，
C、如果子类没有构造方法，则父类无参构造方法作为自已的构造方法，
D、如果子类有无参构造方法，而父类的无参构造方法则被覆盖。

   4.以下程序的运行结果为（  ）
 public class IfTest{
 public static void main(String args[]){
 int x=3;
 int y=1;
 if(x==y)
 System.out.println("Not equal");
 else
 System.out.println("Equal");
 }
 }
     A、Not equal     B、Equal    C、无输出    D、编译出错
     
 5.Java语言中字符串“学Java”所占的内存空间是()        A. 6个字节      B. 7个字节      C. 10个字节      D. 11个字节

6.关于下列程序段的输出结果，说法正确的是：（ ）     public class MyClass{
 static int i;     public static void main(Stringargv[]){     System.out.println(i);     }     }     A、有错误，变量i没有初始化。     B、null        C、1     D、0
 
7.下列哪些语句关于内存回收的说明是正确的? ( )
  A、 程序员必须创建一个线程来释放内存
  B、 内存回收程序负责释放无用内存
  C、 内存回收程序允许程序员直接释放内存
  D、 内存回收程序可以在指定的时间释放内存对象

8.下面异常是属于Runtime Exception 的是（）(多选)
     A、ArithmeticException
     B、IllegalArgumentException
     C、NullPointerException
     D、BufferUnderflowException

9. Math.round(11.5)等于多少(). Math.round(-11.5)等于多少().
     A、11 ,-11   B、11 ,-12   C、12 ,-11   D、12 ,-12

10.下列程序段的输出结果是：（ ）     void complicatedexpression_r(){     int x=20, y=30;     boolean b;     b=x>50&&y>60||x>50&&y<-60||x<-50&&y>60||x<-50&&y<-60;     System.out.println(b);     }     A、true  B、false  C、1  D、0

11.activity对一些资源以及状态的操作保存，最好是保存在生命周期的哪个函数中进行()
   A、onPause()  B、onCreate()   C、 onResume()   D、onStart()

12.Intent传递数据时，下列的数据类型哪些可以被传递（）(多选)
      A、Serializable  B、charsequence  C、Parcelable  D、Bundle

13.android 中下列属于Intent的作用的是()
  A、实现应用程序间的数据共享
  B、是一段长的生命周期，没有用户界面的程序，可以保持应用在后台运行，而不会因为切换页面而消失
  C、可以实现界面间的切换，可以包含动作和动作数据，连接四大组件的纽带
  D、处理一个应用程序整体性的工作

14.下列属于SAX解析xml文件的优点的是()
   A、将整个文档树在内存中，便于操作，支持删除，修改，重新排列等多种功能
      B、不用事先调入整个文档，占用资源少
      C、整个文档调入内存，浪费时间和空间
      D、不是长久驻留在内存，数据不是持久的，事件过后，若没有保存数据，数据就会 消失

 15.下面的对自定style的方式正确的是（）

    A、 <resources>

       <style name="myStyle">

                            <itemname="android:layout_width">fill_parent</item>

                     </style>

 </resources>

     B、 <stylename="myStyle">

                            <itemname="android:layout_width">fill_parent</item>

   </style>

     C、 <resources>

                            <itemname="android:layout_width">fill_parent</item>

  </resources>

     D、 <resources>

                            <stylename="android:layout_width">fill_parent</style>

  </resources>

   16.在android中使用Menu时可能需要重写的方法有（）。(多选)

      A、onCreateOptionsMenu()

  B、onCreateMenu()

  C、onOptionsItemSelected()

  D、onItemSelected()

   17.在SQL Server Management Studio 中运行下列T-SQL语句，其输出值（）。

 SELECT @@IDENTITY 
     A、      可能为0.1
     B、       可能为3
 C、       不可能为-100
     D、      肯定为0

   18.在SQL Server 2005中运行如下T-SQL语句，假定SALES表中有多行数据，执行查询之     后的结果是（）。

 BEGIN TRANSACTION A

       Update SALES Set qty=30WHERE qty<30

        BEGIN TRANSACTION B

              Update SALES Setqty=40 WHERE qty<40

              Update SALES Setqty=50 WHERE qty<50

              Update SALES Setqty=60 WHERE qty<60

        COMMITTRANSACTION B

        COMMIT TRANSACTION A

 A、SALES表中qty列最小值大于等于30

 B、SALES表中qty列最小值大于等于40

 C、SALES表中qty列的数据全部为50

 D、SALES表中qty列最小值大于等于60

   19.在android中使用SQLiteOpenHelper这个辅助类时，可以生成一个数据库，并可以对数      据库版本进行管理的方法可以是()

     A、getWriteableDatabase()

 B、getReadableDatabase()

 C、getDatabase()

 D、getAbleDatabase()

   20. android 关于service生命周期的onCreate()和onStart()说法正确的是()(多选题)

     A、当第一次启动的时候先后调用onCreate()和onStart()方法

     B、当第一次启动的时候只会调用onCreate()方法

     C、如果service已经启动，将先后调用onCreate()和onStart()方法

 D、如果service已经启动，只会执行onStart()方法，不在执行onCreate()方法

   21.下面是属于GLSurFaceView特性的是()(多选)

 A、管理一个surface，这个surface就是一块特殊的内存，能直接排版到android的视图

    view上。

 B、管理一个EGL display，它能让opengl把内容渲染到上述的surface上。

 C、让渲染器在独立的线程里运作，和UI线程分离。

 D、可以直接从内存或者DMA等硬件接口取得图像数据

   22.下面在AndroidManifest.xml文件中注册BroadcastReceiver方式正确的是()

       A、<receiver android:name="NewBroad">

           <intent-filter>

              <action 

                   android:name="android.provider.action.NewBroad"/>

                <action>

           </intent-filter>

       </receiver>

     B、<receiver android:name="NewBroad">

           <intent-filter>

                   android:name="android.provider.action.NewBroad"/>

           </intent-filter>

       </receiver>

     C、<receiver android:name="NewBroad">

           <action 

                  android:name="android.provider.action.NewBroad"/>

             <action>

       </receiver>

     D、<intent-filter>

         <receiver android:name="NewBroad">

               <action>

                   android:name="android.provider.action.NewBroad"/>

                <action>

           </receiver>

</intent-filter>

    23.关于ContenValues类说法正确的是()

      A、他和Hashtable比较类似，也是负责存储一些名值对，但是他存储的名值对当中的

         名是String类型，而值都是基本类型

      B、他和Hashtable比较类似，也是负责存储一些名值对，但是他存储的名值对当中的

         名是任意类型，而值都是基本类型

      C、他和Hashtable比较类似，也是负责存储一些名值对，但是他存储的名值对当中的

         名，可以为空，而值都是String类型     

      D、他和Hashtable比较类似，也是负责存储一些名值对，但是他存储的名值对当中

         的名是String类型，而值也是String类型

24.我们都知道Hanlder是线程与Activity通信的桥梁,如果线程处理不当，你的机器

    就会变得越慢，那么线程销毁的方法是()

       A、onDestroy()

       B、onClear()

       C、onFinish()

       D、onStop()

    25.下面退出Activity错误的方法是（）

       A、finish()

  B、抛异常强制退出

       C、System.exit()

       D、onStop()

     26.下面属于android的动画分类的有()(多项)

       A、Tween  B、FrameC、Draw D、Animation

     27.下面关于Android dvm的进程和Linux的进程,应用程序的进程说法正确的是(d)

        A、DVM指dalivk的虚拟机.每一个Android应用程序都在它自己的进程中运行,不一定拥有一个独立

  的Dalvik虚拟机实例.而每一个DVM都是在Linux 中的一个进程,所以说可以认为是同一个概念.

        B、DVM指dalivk的虚拟机.每一个Android应用程序都在它自己的进程中运行,不一定拥有一个独立

                   的Dalvik虚拟机实例.而每一个DVM不一定都是在Linux 中的一个进程,所以说不是一个概念.

        C、DVM指dalivk的虚拟机.每一个Android应用程序都在它自己的进程中运行,都拥有一个独立的      

           Dalvik虚拟机实例.而每一个DVM不一定都是在Linux 中的一个进程,所以说不是一个概念.

        D、DVM指dalivk的虚拟机.每一个Android应用程序都在它自己的进程中运行,都拥有一个独立的      

           Dalvik虚拟机实例.而每一个DVM都是在Linux 中的一个进程,所以说可以认为是同一个概念.

      28.Android项目工程下面的assets目录的作用是什么

     A、放置应用到的图片资源。

         B、主要放置多媒体等数据文件

         C、放置字符串，颜色，数组等常量数据

         D、放置一些与UI相应的布局文件，都是xml文件

      29..关于res/raw目录说法正确的是(

A、 这里的文件是原封不动的存储到设备上不会转换为二进制的格式

B、 这里的文件是原封不动的存储到设备上会转换为二进制的格式

C、 这里的文件最终以二进制的格式存储到指定的包中

D、 这里的文件最终不会以二进制的格式存储到指定的包中

      30.下列对android NDK的理解正确的是( )

A、 NDK是一系列工具的集合

B、 NDK 提供了一份稳定、功能有限的 API 头文件声明。

C、 使“Java+C” 的开发方式终于转正，成为官方支持的开发方式

D、 NDK 将是 Android 平台支持 C 开发的开端

 

###2．填空题

   1.我们用_______来定义一个整数，用_______来定义一个字符类型，称为原始数据类型。

   2.android中常用的四个布局是__________，__________，__________和__________。

   3.android 的四大组件是__________，__________，__________和__________。

   4.java.io包中的____________和____________类主要用于对对象(Object)的读写。

   5.android 中service的实现方法是：_____________和_____________。

   6.activity一般会重载7个方法用来维护其生命周期，除了onCreate(),onStart(),onDestory()     外还有_____________,______________,____________,___________。

   7.android的数据存储的方式_________,___________,__________,_________,________。

8.当启动一个Activity并且新的Activity执行完后需要返回到启动它的Activity来执行

 的回调函数是_________________。

   9.请使用命令行的方式创建一个名字为myAvd,sdk版本为2.2,sd卡是在d盘的根目录下，

  名字为scard.img， 并指定屏幕大小HVGA.____________________________________。

    10.程序运行的结果是：_______________。

    public classExample{

　　Stringstr=new String("good");

　　char[]ch={'a','b','c'};

　　publicstatic void main(String args[]){

　　　　Exampleex=new Example();

　　　　ex.change(ex.str,ex.ch);

　　　　System.out.print(ex.str+"and ");

　　　　Sytem.out.print(ex.ch);

　　}

　　public voidchange(String str,char ch[]){

　　　　str="testok";

　　　　ch[0]='g';

　　}

}
![选择题答案]
(http://img.blog.csdn.net/20151022170322095)
填空题答案：
1.int char
 2.FrameLayout,LilnearLayout,RelativeLayout,TableLayout
 3.Activity,Broadcast,Service,ContentProvide
 4.ObjectInputStream ObjectOutputSteam
 5.startService ,bindService
 6.onRestart(),onResume(),onPause(),onStop()
 7.SharedPreferences存储，文件存储，SQLite存储，ContentProvider，网络存储
 8.startActivityResult()
 9.adnroid create acd -n myAvd -t 8 -s HVDA - Cd:\card.img
 10.good and gbc



##android笔试——要点

###1.jni的使用
1)安装和下载Cygwin，下载 Android NDK

  2)在ndk项目中JNI接口的设计

  3)使用C/C++实现本地方法

 4)JNI生成动态链接库.so文件

  5)将动态链接库复制到java工程，在java工程中调用，运行java工程即可
 
###2.Activity的生命周期
![Activity生命周期](http://img.blog.csdn.net/20151022170745500)

###3.Android四大组件作用
activity 显示界面（显示的界面都是继承activity完成的）
service 服务（后台运行的，可以理解为没有界面的activity）
Broadcast Receiver 广播（做广播，通知时候用到）
Content Provider  数据通信（数据之间通信，同个程序间数据，或者是不同程序间通信）
###4.Asynctask与Thread区别
如果需要用到与界面相关的线程首选Asynctask（在子线程中进行UI操作），任务完成后通过handler实例向UI线程发送消息，完成界面的更新。
如果线程用来处理数据，不参与界面操作的时候尽量使用thread线程
[Android中AsyncTask的使用与源码分析](http://blog.csdn.net/bboyfeiyu/article/details/8973058)
[Android异步消息处理机制完全解析，带你从源码的角度彻底理解](http://blog.csdn.net/guolin_blog/article/details/9991569)
[Android 异步消息处理机制 让你深入理解 Looper、Handler、Message三者关系](http://blog.csdn.net/lmj623565791/article/details/38377229)

###5.Android像素单位
[Android中的像素单位：dp, sp, px](http://www.cnblogs.com/bluestorm/archive/2012/10/04/2711508.html)

###6.OOM问题
[Android高效加载大图、多图解决方案，有效避免程序OOM](http://blog.csdn.net/guolin_blog/article/details/9316683)

###7.ANR
[什么是ANR 如何避免它？](http://blog.csdn.net/Zengyangtech/article/details/6025671)

###8.事件处理
[Android中的事件处理](http://blog.csdn.net/dawanganban/article/details/19285977)


##算法题
###1. java字符串反转，逆序输出
（句子反转，单词不反转）
[字符串逆序，字母不变（网上的一种方法）](http://blog.csdn.net/jaycee110905/article/details/8882337)
另一种方法，俩次翻转字符串顺序。
I am A—>A ma I—>A am I

```java

public void revese(char[] src,int short,int end)
{
while(start<end)
{
char temp=src[start];
src[start]=src[end];
src[end]=temp;
start++;
end--;
}
}
private void reveseWord(char[] centese)
{
int i=0;
int start=0;
while(i<centese.length)
{
if(centese[i]!='')
{
i++;
}
else{
revese(centese,start,i);
start=i+1;
}
}
}
//在main函数中首次翻转字符串，在调用reveseWord().

```
###2.二进制中1的个数
题目描述
输入一个整数，输出该数二进制表示中1的个数。其中负数用补码表示。

```java
public class Solution {
    public int NumberOf1(int n) {
        int count = 0;
        int flag = 1;
        while (flag != 0) {
            if ((n & flag) != 0) {
                count++;
            }
            flag = flag << 1;//让flag左移，分别和n中的位进行判断
        }
        return count;
      
    }
}

```
###3.首个重复字符
对于一个字符串，请设计一个高效算法，找到第一次重复出现的字符。
给定一个字符串(不一定全为字母)A及它的长度n。请返回第一个重复出现的字符。保证字符串中有重复字符，字符串的长度小于等于500。

```java
publicclassFirstRepeat {
    publiccharfindFirstRepeat(String A, intn) {
        HashSet hs=newHashSet();
        intlength=A.length();
        char[] a=A.toCharArray();
        for(inti=0;i<length;i++)
            {
            booleanb=hs.add(a[i]);//通过往hashset塞值（hashset不准有重复元素），判断当前一段数据中是否有重复元素，一但有，立刻返回
                if(b==false)
                {
returna[i];
            }
        }
        return'0';
        // write code here
    }
}

```
###4.请实现一个函数，将一个字符串中的空格替换成“%20”。例如，当字符串为We Are Happy.则经过替换之后的字符串为We%20Are%20Happy。

```java
public class Solution {
    public String replaceSpace(StringBuffer str) {
        if(str==null)
            {
             return null;
        }
        else
            {
         return str.toString().replace(" ","%20");
        }
    }
}

```
###5.翻转一个string字符串，将单词gooebye翻转。

```java
public class Test {
	public static void main(String[] args)
	{
		String str="GoodBye";
		int length=str.length();
		char[] arr=str.toCharArray();//     把string转换成char型数组
		for(int i=0;i<length;i++)
		{
			if((length-i-1)>=i)
			{
			char temp;
		temp=arr[i];
		arr[i]=arr[length-i-1];
		arr[length-i-1]=temp;
			}
			}
		String string=String.valueOf(arr);//        把char型数组转换成string字符串
		System.out.println(string);
		
	}
}

```

###6.二分法

```java
 /// <summary>
        /// 二分法查找
        /// </summary>
        /// <param name="array">目标数组(已经排序好了)</param>
        /// <param name="a">查找的数</param>
        /// <returns>目标数的索引</returns>
        public int BinarySearch(int[] array, int T)
        {
            int low, high, mid;
            low = 0;
            high = array.Length - 1;
            while (low <= high)
            {
                mid = (low + high) / 2;
                if (array[mid] < T)
                {
                    low = mid + 1;
                }
                else if (array[mid]>T)
                {
                    high = mid - 1;
                }
                else 
                {
                    return mid;
                }
            }
            return -1;
        }

```
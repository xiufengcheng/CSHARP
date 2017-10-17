# 委托和事件
## 引言
### 委托 和 事件在 .Net Framework中的应用非常广泛，然而，较好地理解委托和事件对很多接触C#时间不长的人来说并不容易。它们就像是一道槛儿，过了这个槛的人，觉得真是太容易了，而没有过去的人每次见到委托和事件就觉得心里别（biè）得慌，混身不自在。
### 本次课中，我们将通过若干个范例由浅入深地讲述什么是委托、为什么要使用委托、事件的由来、.Net Framework中的委托和事件、委托和事件对Observer设计模式的意义，对它们的中间代码也做了讨论。

## 委托
### 1. 将方法作为方法的参数
http://www.tracefact.net/CSharp-Programming/Delegates-and-Events-in-CSharp.aspx
- 我们先不管这个标题如何的绕口，也不管委托究竟是个什么东西，来看下面这两个最简单的方法，它们不过是在屏幕上输出一句问候的话语：

-----------------------
```c#
public void GreetPeople(string name) {
    // 做某些额外的事情，比如初始化之类，此处略
    EnglishGreeting(name);
}
public void EnglishGreeting(string name) {
    Console.WriteLine("Morning, " + name);
}
```
-----------------------

- 暂且不管这两个方法有没有什么实际意义。
- GreetPeople方法中将调用EnglishGreeting方法，再次传递name参数
- EnglishGreeting则用于向屏幕输出 “Morning, Jimmy”。
- 现在假设这个程序需要进行汉化，那么，我们再加个中文版的问候方法

```c#
public void ChineseGreeting(string name){
    Console.WriteLine("早上好, " + name);
}
```

-----------------------
- GreetPeople也需要改一改了
- 嗯，定义一个枚举作为判断的依据：
----------------------------
```c#
public enum Language{
    English, Chinese
}

public void GreetPeople(string name, Language lang){
    //做某些额外的事情，比如初始化之类，此处略
    swith(lang){
        case Language.English:
           EnglishGreeting(name);
           break;
       case Language.Chinese:
           ChineseGreeting(name);
           break;
    }
}
```
- 但是，这个解决方案的可扩展性很差
- 如果日后我们需要再添加法文，俄文，韩文、日文版就不得不反复修改枚举和GreetPeople()方法，以适应新的需求。


### 因此，要好好分析一下
- 我们先看看 GreetPeople的方法
```c#
public void GreetPeople(string name, Language lang)
```
- 我们仅看 string name，在这里，string 是参数类型，name 是参数变量
- 当我们赋给name字符串“jimmy”时，它就代表“jimmy”这个值；
- 当我们赋给它“张子阳”时，它又代表着“张子阳”这个值。
- ...
- 假如GreetPeople()方法可以接受一个参数变量，这个变量可以代表另一个方法
- 当我们给这个变量赋值 EnglishGreeting的时候，它代表着 EnglsihGreeting() 这个方法
- 当我们给它赋值ChineseGreeting 的时候，它又代表着ChineseGreeting()方法。
- 我们将这个参数变量命名为makeGreeting
- 那么不是可以如同给name赋值时一样，在调用 GreetPeople()方法的时候，给这个MakeGreeting 参数也赋上值么(ChineseGreeting或者EnglsihGreeting等)？
- 然后，我们在方法体内，也可以像使用别的参数一样使用MakeGreeting。
- 但是，由于MakeGreeting代表着一个方法，它的使用方式应该和它被赋的方法(比如ChineseGreeting)是一样的，比如：
```c#
MakeGreeting(name);
```
- 好了，有了思路了，我们现在就来改改GreetPeople()方法，那么它应该是这个样子了：
```c#
public void GreetPeople(string name, *** MakeGreeting){
    MakeGreeting(name);
}
```
- 那么，这个代表着方法的MakeGreeting参数应该是什么类型的？
### 于是，委托出场了
- 首先，我们看看MakeGreeting参数所能代表的 ChineseGreeting()和EnglishGreeting()方法的签名：
```c#
public void EnglishGreeting(string name)
public void ChineseGreeting(string name)
```
- 我们定义一个委托
```c#
public delegate void GreetingDelegate(string name);
```
- 这里GreetingDelegate表示makeGreeting参数所能代表的方法的类型
- 就像string表示name参数所能代表的值的种类，一样。
```c#
using System;
using System.Collections.Generic;
using System.Text;

namespace Delegate {
     //定义委托，它定义了可以代表的方法的类型
     public delegate void GreetingDelegate(string name);
        class Program {

           private static void EnglishGreeting(string name) {
               Console.WriteLine("Morning, " + name);
           }

           private static void ChineseGreeting(string name) {
               Console.WriteLine("早上好, " + name);
           }

           //注意此方法，它接受一个GreetingDelegate类型的方法作为参数
           private static void GreetPeople(string name, GreetingDelegate makeGreeting) {
               MakeGreeting(name);
            }

           static void Main(string[] args) {
               GreetPeople("Jimmy Zhang", EnglishGreeting);
               GreetPeople("张子阳", ChineseGreeting);
               Console.ReadKey();
           }
        }
    }
```
输出如下：
- Morning, Jimmy Zhang
- 早上好, 张子阳

### 小结
- 委托是一个类，它定义了方法的类型，使得可以将方法当作另一个方法的参数来进行传递，这种将方法动态地赋给参数的做法，可以避免在程序中大量使用if-Else(Switch)语句，同时使得程序具有更好的可扩展性。

### 2. 将方法绑定到委托
- 到这里，你可能会想：不一定要直接在GreetPeople()方法中给name参数赋值，我可以像这样使用变量：
```c#
static void Main(string[] args) {
    string name1, name2;
    name1 = "Jimmy Zhang";
    name2 = "张子阳"; 

     GreetPeople(name1, EnglishGreeting);
     GreetPeople(name2, ChineseGreeting);
    Console.ReadKey();
}
```
- 而既然委托GreetingDelegate和类型string的地位一样，都是定义了一种参数类型，那么，我是不是也可以这么使用委托:
```c#
static void Main(string[] args) {
    GreetingDelegate delegate1, delegate2;
    delegate1 = EnglishGreeting;
    delegate2 = ChineseGreeting;

    GreetPeople("Jimmy Zhang", delegate1);
    GreetPeople("张子阳", delegate2);
        Console.ReadKey();
}
```
- 正是这样！
- 然而，委托不同于string的一个重要特性，**是可以将多个方法赋给同一个委托**(委托绑定多个方法)
```c#
static void Main(string[] args) {
    GreetingDelegate delegate1;
    delegate1 = EnglishGreeting; // 先给委托类型的变量赋值
    delegate1 += ChineseGreeting;   // 给此委托变量再绑定一个方法

     // 将先后调用 EnglishGreeting 与 ChineseGreeting 方法
    GreetPeople("Jimmy Zhang", delegate1);  //或者直接调用  GreetPeople("Jimmy Zhang");
    Console.ReadKey();
}
```

输出为：
- Morning, Jimmy Zhang
- 早上好, Jimmy Zhang


### 小结
- 使用委托可以将多个方法绑定到同一个委托变量，当调用此变量时(这里用“调用”这个词，是因为此变量代表一个方法)，可以依次调用所有绑定的方法。


--------------------------------------
### 课堂练习：现在有两个函数
```c#
 static double Multiply(double param1, double param2)
      {
         return param1 * param2;
      }
 static double Divide(double param1, double param2)
      {
         return param1 / param2;
      }
 ```



试编写一个完整的cs文件，使得这一运作成形

------------------------
答案：

```c#
#region Using directives

using System;
using System.Collections.Generic;
using System.Text;

#endregion

namespace Ch06Ex05
{
   class Program
   {
      delegate double ProcessDelegate(double param1, double param2);//定义委托

      static double Multiply(double param1, double param2)
      {
         return param1 * param2;
      }

      static double Divide(double param1, double param2)
      {
         return param1 / param2;
      }

      static void Main(string[] args)
      {
         ProcessDelegate process; //定义委托变量
         Console.WriteLine("Enter 2 numbers separated with a comma:");
         string input = Console.ReadLine();
         int commaPos = input.IndexOf(',');
         double param1 = Convert.ToDouble(input.Substring(0, commaPos));
         double param2 = Convert.ToDouble(input.Substring(commaPos + 1,
                                                 input.Length - commaPos - 1));
         Console.WriteLine("Enter M to multiply or D to divide:");
         input = Console.ReadLine();
         if (input == "M")
            process = new ProcessDelegate(Multiply);//或写成：process = Multiply;
         else
            process = new ProcessDelegate(Divide);  //或写成：process = Divide;
         Console.WriteLine("Result: {0}", process(param1, param2));
         Console.ReadKey();
      }
   }
}
```



## 事件
### 1. 事件是应用程序开发中最常用的概念
- 所有与用户交互的过程基本上都是事件触发和处理的过程
- 例如：单击鼠标左键，双击鼠标左键….Windows From或Web窗体中的按钮事件等…
- 又例如：有一个类Animals,生成一个tiger对象和一个sheep对象，当把tiger对象添加到一个数组中的时候，所用的add()方法既不存在于Animals类中，也不存在与它的父类或子类中，而让tiger去吃sheep的那个方法eat()，也不存在于Animals类中，因此，需要给代码添加事件处理程序


### 2. 事件处理程序是一种特殊类型的函数
- 在事件发生的时候调用，还需要配置这个处理程序，以监听自己感兴趣的事件。


### 3. 事件是一种特殊的委托

### 4. 事件具有如下特性
- 事件就是一种消息，其本质是对消息的封装。
- 事件用作对象之间的通信。
- 事件的发起方称为事件发送器/事件接收方称为事件接收器。
- 事件发送器触发了一个事件，但并不知道哪个对象或方法会收到并处理它所触发的事件，所需要的是在发送方和接收方之间存在一个媒介，这个媒介就是委托。

### 5. C#中预定义的委托EventHandler
- C#中提供了一个名为EventHandler的预定义委托, 该委托专门用于不生成数据的事件(鼠标等)处理程序的方法。
```c#
public delegate void EventHandler(   
       Object sender;                      //触发器
       EventArgs   e;                      //事件参数
)
```
- 其中，第一个参数类型为Object，它引用引发事件的实例；
- 第二个参数从EventArgs类型派生，它保存事件数据。

### 6. 向EventHandler中添加委托
- 若要将事件与处理事件的方法相关联，还要向事件添加委托的实例：
- 这时需要使用关键字event来声明，例如：
```c#
public   event  EventHandler   NoDataEventHandler;
```

### 7. 定义事件处理程序
- 定义了事件成员，就可以吧事件处理的相关程序关联到事件上。事件处理程序委托会被绑定到系统引发事件时要执行的方法。
- 事件处理程序会被添加到事件中，以便当事件引发时，事件处理程序能够调用它的方法，这就是**订阅事件**。过程如下：
- (1)编写事件处理程序(订阅)，例如：
```c#
void  HandleCustomEvent(Object sender, CustomEventArgs  a)
 {
        //处理程序
 }
 ```
- (2)使用+=来为事件订阅，例如
```c#
publisher.RaiseCustomEvent += HandleCustomEvent;
```

- 整个流程如下
```c#
     //产生事件实例
     public class A   
    {   //定义委托，名称为EventHandler，无返回值
        public delegate void EventHandler(object sender,EventArgs e);
        
       public event EventHandler a;               //声明事件
        
       public void Run(EventArgs e)               //引发事件
        
       {
            Console.WriteLine("产生一个事件。");
            a(this, e);//处理当前引发的事件
        
        }
    }

    //接收事件实例
    class B
    { 
        public B(A a)
        {
                a.a += new A.EventHandler(this.b);      //订阅事件处理程序
        }
        
        private void b(object sender, EventArgs e)    //事件处理方法
        {
            Console.WriteLine("处理事件！");
            Console.Read();
        }
    }

    class Program
    {
        static void Main(string[] args)
        {
            A a = new A();
            B b = new B(a);
            EventArgs e = new EventArgs();
            a.Run(e);//触发事件，并调用事件处理程序
 
        }
    }

```

### 8. EventArgs类派生
- EventArgs类是所有事件类的基类, 本身并不包含事件数据。
- 不向事件处理程序传递信息的时候，会用到此类。
- 如果事件处理程序需要状态信息，则应用程序必须从EventArgs类派生的一个类来保存数据，例如：
```c#
internal class KeyEventArgs:EventArgs
{
      private char keyChar;//按键信息
      public KeyEventArgs(char keyChar):base()
             { this.keyChar = keyChar;}
     public char keyChar  {  get {return keyChar;}   }
}
```

下面实现一个火警类FireAlarm，它将处罚火警事件。
```c#
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;

namespace Sample10_6
{
    public class FireEventArgs : EventArgs//派生自EventArgs类，表示火警事件
    {
        public FireEventArgs(string room, int ferocity)//构造函数
        {
            this.room = room;
            this.ferocity = ferocity;
        }

        //火警事件包含两方面信息
        public string room;//火警发生地
        public int ferocity;//火势等级

    }
    //火警类
    public class FireAlarm
    {
        //定义委托，第2个参数为火警事件FireEventArgs类型
        public delegate void FireEventHandler(object sender, FireEventArgs fe);

        // 创建火警事件

        public event FireEventHandler FireEvent;

        //触发火警事件
        public void ActivateFireAlarm(string room, int ferocity)
        {

            FireEventArgs fireArgs = new FireEventArgs(room, ferocity);

           //调用委托

            FireEvent(this, fireArgs);
        }
    }	
    //火警事件处理类
    class FireHandlerClass
    {

        //传入火警事件对象
        public FireHandlerClass(FireAlarm fireAlarm)
        {

           //订阅火警事件处理程序
            fireAlarm.FireEvent += new FireAlarm.FireEventHandler(ExtinguishFire);
        }

        //火警事件处理程序
        void ExtinguishFire(object sender, FireEventArgs fe)
        {

            Console.WriteLine("\n火警事件是由{0}引发的：", sender.ToString());
            //对火警事件进行处理
            if (fe.ferocity < 2)
                Console.WriteLine("发生在{0}的火警是没有问题的. 用水就可以浇灭.", fe.room);
            else if (fe.ferocity < 5)
                Console.WriteLine("要使用灭火器才能扑灭{0}的大火.", fe.room);
            else
                Console.WriteLine("发生在{0}的大火已经失控，请通知政府部门!", fe.room);
        }
    }
    class Program
    {
        static void Main(string[] args)
        {
            //创建火警对象
            FireAlarm myFireAlarm = new FireAlarm();
            //创建火警事件处理程序对象
            FireHandlerClass myFireHandler = new FireHandlerClass(myFireAlarm);

            //触发一些火警事件
            myFireAlarm.ActivateFireAlarm("厨房", 3);
            myFireAlarm.ActivateFireAlarm("书房", 1);
            myFireAlarm.ActivateFireAlarm("车库", 5);

            Console.ReadLine();


        }
    }
}
```

### 9. 实现接口事件
```c#


### 10. 实现winform窗体事件
```c#
#region Using directives

using System;
using System.Collections.Generic;
using System.ComponentModel;
using System.Data;
using System.Drawing;
using System.Windows.Forms;

#endregion

namespace Ch08Ex01
{
   partial class Form1 : Form
   {
      public Form1()
      {
         InitializeComponent();
      }

      private void button1_Click(object sender, EventArgs e)
      {
         ((Button)sender).Text = "Clicked!";
         Button newButton = new Button();
         newButton.Text = "New Button!";
         newButton.Click += new EventHandler(newButton_Click);
         Controls.Add(newButton);
      }

      private void newButton_Click(object sender, System.EventArgs e)
      {
         ((Button)sender).Text = "Clicked!!";
      }
   }
}



```
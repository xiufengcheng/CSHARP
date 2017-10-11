## 类的继承1:Truck 和 Bus 继承 Automobile
```c#
using System;
namespace P5_1
{
    class Program
    {
        public static void Main()
        {
            Bus b1 = new Bus();
			//b1.speed = 80;
            Console.WriteLine("客车行驶1000公里需{0}小时", b1.Run(1000));
            Truck t1 = new Truck();
            Console.WriteLine("卡车行驶1000公里需{0}小时", t1.Run(1000));
            Console.ReadLine();
        }
    }

    public class Automobile
    {
        protected float speed;
        public float Speed
        {
            get { return speed; }
        }

        private float weight;
        public float Weight
        {
            get { return weight; }
            set { weight = value; }
        }

        public float Run(float distance)   //此为基类中的run方法
        {
            return distance / speed;
        }
    }

    public class Bus : Automobile
    {
        private int passengers;
        public int Passengers
        {
            get { return passengers; }
            set { passengers = value; }
        }

        public Bus()
        {
            passengers = 20;
            speed = 60;
            Weight = 10;
        }
    }

    public class Truck : Automobile
    {
        private float load;
        public float Load
        {
            get { return load; }
            set { load = value; }
        }

        public Truck()
        {
            load = 30;
            speed = 50;
            Weight = 15;
        }
    }
}
```

## 类的继承2:Truck 和 Bus 继承 Automobile, 但是Turck的run()函数覆盖了基类中的run
```c#
using System;
namespace P5_2
{
    class Program
    {
        static void Main()
        {
            Truck t1 = new Truck();
            Console.WriteLine("卡车速度{0}公里/小时", t1.Speed);
            Console.WriteLine("卡车行驶1000公里需{0}小时", t1.Run(1000));
            Automobile a1 = t1;
            Console.WriteLine("汽车速度{0}公里/小时", a1.Speed);
            Console.WriteLine("汽车行驶1000公里需{0}小时", a1.Run(1000));
            Console.ReadLine();
        }
    }

    public class Automobile
    {
        protected float speed;
        public float Speed
        {
            get { return speed; }
        }

        private float weight;
        public float Weight
        {
            get { return weight; }
            set { weight = value; }
        }

        public float Run(float distance)
        {
            return distance / speed;
        }
    }

    public class Truck : Automobile
    {
        private float load;
        public float Load
        {
            get { return load; }
            set { load = value; }
        }

        public new float Speed
        {
            get { return speed / (1 + load / Weight / 2); }
        }

        public Truck()
        {
            load = 30;
            speed = 50;
            Weight = 15;
        }

        public new float Run(float distance)       //此为隐藏基类的派生类方法
        {
            return (1 + load / Weight / 2) * base.Run(distance);
        }
    }
}
```

## 类的继承3:base关键字
```c#
 public class Truck : Automobile
    {
        //...
        public void ShowSpeed()
        {
           Truck t1 = new Truck();   //访问基类的属性和方法
           Console.WriteLine("空载速度:{0}", base.Speed);
           Console.WriteLine("行驶1000公里需{0}小时", base.Run(1000));

           Console.WriteLine("空载速度:{0}", Speed);    //访问自身属性
           Console.WriteLine("行驶1000公里需{0}小时", Run(1000));
        }
    }
```

## 对象的生命周期
```c++
using System;
namespace P5_3
{
    class Program
    {
        public static void Main()
        {
            Son s1 = new Son();
            System.GC.Collect();
        }
    }

    public class Grandsire
    {
        public Grandsire()
        {  Console.WriteLine("调用Grandsire的构造函数");   }

        ~Grandsire()
        {  Console.WriteLine("调用Grandsire的析构函数");   }
    }

    public class Father : Grandsire
    {
        public Father()
        {   Console.WriteLine("调用Father的构造函数");     }

        ~Father()
        {   Console.WriteLine("调用Father的析构函数");     }
    }

    public class Son : Father
    {
        public Son()
        {   Console.WriteLine("调用Son的构造函数");        }

        ~Son()
        {   Console.WriteLine("调用Son的析构函数");        }
    }
}
```


## 用虚拟和重载的方法实现多态
C#中，只要将基类的方法定义为虚拟方法(Virtual)，将派生类中对应的方法定义为重载方法(override)，那么程序就能根据对象的实际类型来决定调用哪一个方法。
```c++
using System;
namespace P5_4
{
    class Program
    {
        static void Main()
        {
            foreach (Automobile a in GetAutos())
            {
                a.Speak();
                Console.WriteLine(a.ToString());
                Console.WriteLine("{0}行驶1000公里需{1}小时", a.Name, a.Run(1000));
            }
            Console.ReadLine();
        }

        static Automobile[] GetAutos()
        {
            Automobile[] autos = new Automobile[4];
            autos[0] = new Bus("客车", 20);
            autos[1] = new Truck("东风卡车", 30);
            autos[2] = new Truck("黄河卡车", 45);
            autos[3] = new Automobile("汽车", 80, 3);
            return autos;
        }
    }

    public class Automobile
    {
        private string name;
        public string Name
        {  get { return name; }  }

        private float speed;
        public float Speed
        {  get { return speed; }  }

        private float weight;
        public float Weight
        {  get { return weight; }
           set { weight = value; }
        }

        public Automobile(string name, float speed, float weight)
        {
            this.name = name;
            this.speed = speed;
            this.weight = weight;
        }

        public virtual float Run(float distance) //虚拟方法
        {     return distance / speed;   }

        public virtual void Speak() //虚拟方法
        {   Console.WriteLine("汽车鸣笛...");     }

        public override string ToString()
        {   return name;    }
    }

    public class Bus : Automobile
    {
        private int passengers;
        public int Passengers
        {
            get { return passengers; }
            set { passengers = value; }
        }

        public Bus(string name, int passengers)
            : base(name, 60, 10)
        {
            this.passengers = passengers;
        }

        public override void Speak() //重载方法
        {
            Console.WriteLine("嘀...嘀....");
        }
    }

    public class Truck : Automobile
    {
        private float load;
        public float Load
        {
            get { return load; }
            set { load = value; }
        }

        public Truck(string name, int load)
            : base(name, 50, 15)
        {
            this.load = load;
        }

        public override float Run(float distance) //重载方法
        {
            return (1 + load / Weight / 2) * base.Run(distance);
        }

        public override void Speak() //重载方法
        {
            Console.WriteLine("叭...叭...");
        }
    }
}
```
## 用抽象方法实现多态
现实生活中，有很多类属于抽象类，因为其本身不与具体的对象相联系，但可以为其派生类提供一个公共的界面，例如，“交通工具”，“图形”等等，可以做为抽象类，而“三角形”不应是一个抽象类，因为其对象既可以是派生的“等腰三角形”，“直角三角形”等特殊三角形，也可以是一般的三角形。
```c#
using System;
namespace P5_5
{
    class Program
    {
        static void Main()
        {
            Vehicle v1 = new Train();
            v1.Speak();
            Console.WriteLine("行驶1000公里需{0}小时", v1.Run(1000));
            v1 = new Truck(16, 24);
            v1.Speak();
            Console.WriteLine("行驶1000公里需{0}小时", v1.Run(1000));
            Console.ReadLine();
        }
    }

    public abstract class Vehicle
    {
        private float speed;
        public float Speed
        {
            get { return speed; }
        }

        public Vehicle(float speed)
        {
            this.speed = speed;
        }

        public virtual float Run(float distance) //虚拟方法
        {
            return distance / speed;
        }

        public abstract void Speak(); //抽象方法：无执行代码
    }

    public class Train : Vehicle
    {
        public Train()
            : base(160)
        { }

        public override void Speak() //重载
        {
            Console.WriteLine("呜......");
        }
    }

    public abstract class Automobile : Vehicle
    {
        public Automobile(float speed)
            : base(speed)
        { }

        public override abstract void Speak(); //重载+抽象
    }

    public class Truck : Automobile
    {
        private float weight;
        public float Weight
        {
            get { return weight; }
        }

        private float load;
        public float Load
        {
            get { return load; }
        }

        public Truck(int weight, int load)
            : base(50)
        {
            this.weight = weight;
            this.load = load;
        }

        public override float Run(float distance) //重载
        {
            return (1 + load / Weight / 2) * base.Run(distance);
        }

        public override void Speak() //重载
        {
            Console.WriteLine("叭...叭...");
        }
    }
}
```

## 接口与抽象类的区别
```c#
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;

namespace Sample8_1
{
    class Program
    {
        static void Main(string[] args)
        {
            Car car = new Car("小汽车");
            car.Show();
            car.Move();
            car.GetWheel();
            Plane plane = new Plane("飞机");
            plane.Show();
            plane.Move();
            plane.GetWheel();
            Console.ReadLine();
        }
    }

    //定义抽象类，表示交通工具
    abstract class Travel
    {
        protected string _name;//交通工具的名称

        public abstract string Name//定义抽象属性，表示交通工具的名称
        {
            get;
            set;
        }

        public void Show()//定义一般方法，用来显示是何种交通工具
        {
            Console.WriteLine("这是{0}",_name);
        }
        public abstract void GetWheel();//获取轮子的数量
    }

    //定义接口
    interface IAction
    {
        //注意这个方法同抽象类中的方法GetWheel的区别
        //任何东西都可以具有移动行为，而只有交通工具才有轮子
        void Move();//表示交通工具行驶的行为
    }
    class Car : Travel, IAction
    {
        public override string Name//重写抽象类属性
        {
            get
            {
                return _name;
            }
            set
            {
                _name = value;
            }
        }
        public Car(string name)
        {
            _name = name;
        }
        public void Move()//实现接口方法
        {
            Console.WriteLine("小汽车行走在公路上");
        }

        public override void GetWheel()//重写抽象类方法
        {
            Console.WriteLine("小汽车有4个轮子");
        }
    }
    class Plane : Travel, IAction
    {
        public override string Name//重写抽象类属性
        {
            get
            {
                return _name;
            }
            set
            {
                _name = value;
            }
        }
        public Plane(string name)
        {
            _name = name;
        }
        public void Move()//实现接口方法
        {
            Console.WriteLine("飞机在空中飞行");
        }

        public override void GetWheel()//重写抽象类方法
        {
            Console.WriteLine("飞机有6个轮子");
        }
    }
}
```


## 接口的实现
定义了一个英制单位接(IEnglishDimensions)和一个公制单位(IMetricDimensions)，分别都有长度Lenghth和宽度Width两个属性，实现一个Box类，显示实现这两个接口的成员。

```c#
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;

namespace Sample8_2
{
    class Program
    {
        static void Main(string[] args)
        {
            // 声明类实例“myBox”：
            Box myBox = new Box(30.0f, 20.0f);
            // 声明英制单位接口的实例：
            IEnglishDimensions eDimensions = (IEnglishDimensions)myBox;
            // 声明公制单位接口的实例：
            IMetricDimensions mDimensions = (IMetricDimensions)myBox;
            // 以英制单位打印尺寸：
            System.Console.WriteLine("Length(英寸): {0}", eDimensions.Length);
            System.Console.WriteLine("Width (英寸): {0}", eDimensions.Width);
            // 以公制单位打印尺寸：
            System.Console.WriteLine("Length(厘米): {0}", mDimensions.Length);
            System.Console.WriteLine("Width (厘米): {0}", mDimensions.Width);
            Console.ReadLine();
        }
    }

    // 声明英制单位接口：
    interface IEnglishDimensions
    {
        float Length
        {
            get;
            set;
        }
        float Width
        {
            get;
            set;
        }
    }
    // 声明公制单位接口：
    interface IMetricDimensions
    {
        float Length
        {
            get;
            set;
        }
        float Width
        {
            get;
            set;
        }
    }
    // 声明实现以下两个接口的“Box”类：
    // IEnglishDimensions 和 IMetricDimensions：
    class Box : IEnglishDimensions, IMetricDimensions 
    {
       float lengthInches;
       float widthInches;
       public Box(float length, float width)
       {
           lengthInches = length;
           widthInches = width;
       }
       float IEnglishDimensions.Length//实现IEnglishDimensions
       {
           get
           {
               return lengthInches;
           }
           set
           {
               lengthInches = value;
           }
       }

       float IEnglishDimensions.Width//实现IEnglishDimensions
       {
           get
           {
               return widthInches;
           }
           set
           {
               widthInches = value;
           }
       }

       float IMetricDimensions.Length//实现IMetricDimensions
       {
           get
           {
               return lengthInches * 2.54f;
           }
           set
           {
               lengthInches = value * 2.54f;
           }
       }

       float IMetricDimensions.Width//实现IMetricDimensions
       {
           get
           {
               return widthInches * 2.54f;
           }
           set
           {
               widthInches = value * 2.54f;
           }
       }
    }
}
```
## 异常
```c#
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;

namespace Sample6_1
{
    class Program
    {
        static void Main(string[] args)
        {
            try
            {
                int a, b = 0;
                a = 20 / b;//除数为0
            }
            catch(Exception e)//处理异常
            {
                //获取描述当前异常的消息
                Console.WriteLine("当前异常:{0}",e.Message);
                //获取或设置导致错误的应用程序或对象的名称
                Console.WriteLine("导致异常的对象:{0}", e.Source);
                //获取调用堆栈上直接帧的字符串表示形式
                Console.WriteLine("字符串表示形式:{0}", e.StackTrace);
                //获取引发当前异常的方法
                Console.WriteLine("引发当前异常的方法:{0}", e.TargetSite);
            }
            Console.ReadKey();
            
        }
    }
}
```
## 异常：异常类
```c#
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;

namespace Sample6_2
{
    class Program
    {
        static void Main(string[] args)
        {
            try
            {
                int a, b = 0;
                a = 20 / b;//除数为0
            }
            catch (DivideByZeroException e)//处理异常
            {
                //获取描述当前异常的消息
                Console.WriteLine("当前异常:{0}", e.Message);
                //获取或设置导致错误的应用程序或对象的名称
                Console.WriteLine("导致异常的对象:{0}", e.Source);
                //获取调用堆栈上直接帧的字符串表示形式
                Console.WriteLine("字符串表示形式:{0}", e.StackTrace);
                //获取引发当前异常的方法
                Console.WriteLine("引发当前异常的方法:{0}", e.TargetSite);
            }
            Console.ReadKey();
        }
    }
}
```
## 异常：finally
```c#
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;

namespace Sample6_4
{
    class Program
    {
        static void CodeWithFinally()
        {
            //这个例子中使用的文件对象将在后续章节介绍
            System.IO.FileStream file = null;//定义文件流
            System.IO.FileInfo fileInfo = null;//定义文件

            try
            {
                //尝试打开文件
                fileInfo = new System.IO.FileInfo("C:\\file.txt");
                file = fileInfo.OpenWrite();
                file.WriteByte(0xF);
            }
                //处理异常
            catch (System.UnauthorizedAccessException e)
            {
                System.Console.WriteLine(e.Message);
            }
            finally//关闭使用过的文件对象
            {
                if (file != null)
                {
                    file.Close();
                }
            }
        }
        static void Main(string[] args)
        {
            CodeWithFinally();
            Console.ReadKey();
        }
    }
}
```

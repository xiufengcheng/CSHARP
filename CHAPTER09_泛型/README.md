## 用系统定义好的泛型类
```c#
using System;
using System.Collections.Generic;

namespace P12_1
{
    class Program
    {
        static void Main()
        {
            int[] x1 = { 5, 9, 6, 3, -6, 12 };
            Stack<int> stack1 = new Stack<int>();
            for (int i = 0; i < x1.Length; i++)
                stack1.Push(x1[i]);
            int[] x2 = new int[x1.Length];
            for (int i = 0; i < x2.Length; i++)
                x2[i] = stack1.Pop();
            foreach (int i in x2)
                Console.WriteLine(i);
            string[] ss1 = { "one", "day", "when", "we", "were", "young" };
            Stack<string> stack2 = new Stack<string>();
            for (int i = 0; i < ss1.Length; i++)
                stack2.Push(ss1[i]);
            string[] ss2 = new string[ss1.Length];
            for (int i = 0; i < ss1.Length; i++)
                ss2[i] = stack2.Pop();
            foreach (string s in ss2)
                Console.WriteLine(s);
            Console.ReadLine();
        }
    }
}
```
## 自定义的泛型类
```c#
using System;

namespace P12_2
{
    public class LinkNode<T>
    {
        protected T data;
        protected LinkNode<T> next;

        public T Data // 获取或设置节点值
        {
            get { return data; }
            set { data = value; }
        }

        public LinkNode<T> Next // 获取或设置节点的下一个节点
        {
            get { return next; }
            set { next = value; }
        }

        public LinkNode() // 创建链表节点
        {
            data = default(T);
        }

        public LinkNode(T t) // 指定数据创建链表节点
        {
            data = t;
        }

        public static LinkNode<T> operator >>(LinkNode<T> node, int n) //移至node的第n个后续节点
        {
            LinkNode<T> node1 = node;
            for (int i = 0; i < n && node1 != null; i++)
                node1 = node1.next;
            return node1;
        }
    }
}
```



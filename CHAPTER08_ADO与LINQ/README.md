## 实验一：访问数据库

```c#
INSERT INTO dbo.[Table] VALUES(123,'Gates','1234567');

INSERT INTO dbo.[Table] VALUES(222,'Mike','1234567');

UPDATE dbo.[Table] SET Telephone = '88888' WHERE Stuname = 'Mike';

SELECT * FROM dbo.[Table];

DELETE FROM dbo.[Table] WHERE Id=123;


DELETE FROM dbo.[USER] WHERE username = 'CHENG';

SELECT * FROM dbo.[USER];

//等等SQL语句
```


## 实验二：实现用户登录
```c#
[in Form1.cs]
using System;
using System.Collections.Generic;
using System.ComponentModel;
using System.Data;
using System.Data.SqlClient;
using System.Drawing;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using System.Windows.Forms;

namespace loginaccount
{
    public partial class Form1 : Form
    {
        public Form1()
        {
            InitializeComponent();
        }

        private void button1_Click(object sender, EventArgs e)
        {
            try
            {
                string UserName = textBox1.Text.Trim();
                string PassWord = textBox2.Text.Trim();
                if (UserName == "")
                {
                    MessageBox.Show("用户名为空，请输入用户名", "提示", MessageBoxButtons.OK, MessageBoxIcon.Information);
                }
                else if (PassWord == "")
                {
                    MessageBox.Show("密码为空，请输入密码", "提示", MessageBoxButtons.OK, MessageBoxIcon.Information);

                }
                else
                {
                    string str = "SELECT count(*) from Users WHERE username='" + UserName + "' and password = '" + PassWord + "'";
                    string connectionStr = "Data Source=(local)\\SQLEXPRESS;Initial Catalog=MYDB;Integrated Security=True";
                    SqlConnection conn = new SqlConnection(connectionStr); //创建连接对象
                    SqlCommand cmd = new SqlCommand(str, conn);//创建命令对象
                    conn.Open();  //如果一个连接对象打开，那么必须在try最后关闭它
                    Console.WriteLine("数据连接成功打开");

                    SqlDataReader sdr = cmd.ExecuteReader(); //创建数据读取器对象
                    sdr.Read();
                    sdr.Close();
                    int n = (int)cmd.ExecuteScalar(); //传回第一行，赋给n
                    if (n >= 1)
                    {
                        MessageBox.Show("SUCCESS!");
                    }
                    else {
                        MessageBox.Show("用户名或密码错误", "警告", MessageBoxButtons.OK, MessageBoxIcon.Warning);//弹出警告提示，密码不对
                    }
                    conn.Close();
                }
            }
            catch (Exception ex)
            {
                MessageBox.Show(ex.Message);//打印异常
            }
        }
    }
}
```
using System;
using System.Collections.Generic;
using System.ComponentModel;
using System.Data;
using System.Drawing;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using System.Windows.Forms;

namespace game
{
    public partial class Form1 : Form
    {
        public Form1()
        {
            InitializeComponent();
            textBoxmin.MaxLength = 9;
            textBoxmax.MaxLength = 9;
        }

        private Random number = new Random();

        public int answer;
        public int min;
        public int max;
        public int i;

        private void BtnPassword_Click(object sender, EventArgs e) //產生密碼
        {
            i = 0;
            string n = textBoxmin.Text;
            string x = textBoxmax.Text;
            bool result = int.TryParse(n, out i) && int.TryParse(x, out i);  //判斷最大值與最小值是否為整數

            if (result == false || int.Parse(textBoxmin.Text) < 0 || int.Parse(textBoxmax.Text) < 0)  // 不是整數
            {
                MessageBox.Show("輸入錯誤! 請輸入正整數!");
                textBoxmin.Clear();
                textBoxmax.Clear();
                textBoxmin.Focus();
            }
            else if (result == true) // 是整數
            {
                min = int.Parse(textBoxmin.Text);
                max = int.Parse(textBoxmax.Text);

                if (max <= min || max <= (min + 2)) //判斷輸入條件是否正確
                {
                    if (max < min)  //當最大值<最小值
                    {
                        MessageBox.Show("輸入錯誤! 最大值不可低於最小值");
                    }
                    else if (max == min) //當最大值=最小值
                    {
                        MessageBox.Show("輸入錯誤! 最大值與最小值1不可相等");
                    }
                    else if (max <= (min + 2))  //當最大值<=最小值+2
                    {
                        MessageBox.Show("輸入錯誤! 最大值與最小值相差需大於2");
                    }

                    textBoxmin.Clear();
                    textBoxmax.Clear();
                    textBoxmin.Focus();
                }
                else if (max > min) //符合產生條件
                {
                    Lbmin.Text = min.ToString();
                    Lbmax.Text = max.ToString();

                    answer = number.Next(min + 1, max);  //產生密碼
                    BtnPassword.Enabled = false;
                    textBoxmin.Enabled = false;
                    textBoxmax.Enabled = false;
                    textBox.Enabled = true;
                    RadioBtnFriend.Enabled = false;
                    radioBtnComputer.Enabled = false;
                    textBox.Focus();
                }
            }
        }

        private void BtnOK_Click(object sender, EventArgs e) // OK鍵
        {
            i = 0;
            string t = textBox.Text;
            bool result = int.TryParse(t, out i);  //判斷輸入的值是否為正整數

            if (result == false)
            {
                MessageBox.Show("輸入錯誤!請輸入正整數!");
                textBox.Clear();
                textBox.Focus();
            }
            else if (result == true && RadioBtnFriend.Checked == true)  // 真人模式
            {
                if (int.Parse(textBox.Text) >= int.Parse(Lbmax.Text) || int.Parse(textBox.Text) <= int.Parse(Lbmin.Text)) //偵測輸入範圍
                {
                    MessageBox.Show("輸入錯誤!請輸入範圍內的值!");
                    textBox.Clear();
                    textBox.Focus();
                }
                else if (int.Parse(textBox.Text) < answer)  // 當輸入的值小於密碼
                {
                    Lbmin.Text = textBox.Text;
                    MessageBox.Show(Lbmin.Text + "~" + Lbmax.Text);
                    textBox.Clear();
                    textBox.Focus();
                }
                else if (int.Parse(textBox.Text) > answer) // 當輸入的值大於密碼
                {
                    Lbmax.Text = textBox.Text;
                    MessageBox.Show(Lbmin.Text + "~" + Lbmax.Text);
                    textBox.Clear();
                    textBox.Focus();
                }
                else if (int.Parse(textBox.Text) == answer) // 當輸入的值等於密碼
                {
                    MessageBox.Show("Congratulations! 答案就是 " + answer);
                    BtnReset.PerformClick();
                }
            }
            else if (result == true && RadioBtnFriend.Checked == false) // 電腦對戰模式
            {
                if (int.Parse(textBox.Text) >= int.Parse(Lbmax.Text) || int.Parse(textBox.Text) <= int.Parse(Lbmin.Text)) //偵測輸入範圍
                {
                    MessageBox.Show("輸入錯誤!請輸入範圍內的值!");
                    textBox.Text = "0";
                    textBox.Focus();
                }
                // 真人作答
                else if (int.Parse(textBox.Text) < answer)  // 當輸入的值小於密碼
                {
                    Lbmin.Text = textBox.Text;
                    MessageBox.Show(Lbmin.Text + "~" + Lbmax.Text);
                    MessageBox.Show("換電腦囉~");
                    textBox.Focus();
                }
                else if (int.Parse(textBox.Text) > answer) // 當輸入的值大於密碼
                {
                    Lbmax.Text = textBox.Text;
                    MessageBox.Show(Lbmin.Text + "~" + Lbmax.Text);
                    MessageBox.Show("換電腦囉~");

                    textBox.Focus();
                }
                else if (int.Parse(textBox.Text) == answer) // 當輸入的值等於密碼
                {
                    MessageBox.Show("Congratulations! \n\r答案就是 " + answer);
                    BtnReset.PerformClick();
                    textBox.Text = "0";
                }
                if (int.Parse(textBox.Text) != answer && int.Parse(textBox.Text) != 0)
                {
                    int cmpnumber = number.Next((int.Parse(Lbmin.Text)) + 1, int.Parse(Lbmax.Text));

                    //電腦作答

                    if (cmpnumber < answer)  // 當輸入的值小於密碼
                    {
                        Lbmin.Text = cmpnumber.ToString();
                        MessageBox.Show("電腦猜:" + cmpnumber);
                        MessageBox.Show(Lbmin.Text + "~" + Lbmax.Text);
                        textBox.Clear();
                        textBox.Focus();
                    }
                    else if (cmpnumber > answer) // 當輸入的值大於密碼
                    {
                        Lbmax.Text = cmpnumber.ToString();
                        MessageBox.Show("電腦猜:" + cmpnumber);
                        MessageBox.Show(Lbmin.Text + "~" + Lbmax.Text);
                        textBox.Clear();
                        textBox.Focus();
                    }
                    else if (cmpnumber == answer) // 當輸入的值等於密碼
                    {
                        MessageBox.Show("電腦猜:" + answer);
                        MessageBox.Show("電腦獲勝!\n\r答案就是 " + answer);
                        BtnReset.PerformClick();
                    }
                }
                textBox.Clear();
                textBox.Focus();
            }
        }

        private void BtnReset_Click(object sender, EventArgs e) //重新開始
        {
            textBox.Clear();
            textBoxmin.Clear();
            textBoxmax.Clear();
            Lbmin.Text = "";
            Lbmax.Text = "";
            BtnPassword.Enabled = true;
            textBoxmin.Enabled = true;
            textBoxmax.Enabled = true;
            textBox.Enabled = false;
            RadioBtnFriend.Enabled = true;
            radioBtnComputer.Enabled = true;
            textBoxmin.Focus();
        }

        private void BtnClose_Click(object sender, EventArgs e)
        {
            this.Close();
        }
    }
}
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

        private void BtnPassword_Click(object sender, EventArgs e) //���ͱK�X
        {
            i = 0;
            string n = textBoxmin.Text;
            string x = textBoxmax.Text;
            bool result = int.TryParse(n, out i) && int.TryParse(x, out i);  //�P�_�̤j�ȻP�̤p�ȬO�_�����

            if (result == false || int.Parse(textBoxmin.Text) < 0 || int.Parse(textBoxmax.Text) < 0)  // ���O���
            {
                MessageBox.Show("��J���~! �п�J�����!");
                textBoxmin.Clear();
                textBoxmax.Clear();
                textBoxmin.Focus();
            }
            else if (result == true) // �O���
            {
                min = int.Parse(textBoxmin.Text);
                max = int.Parse(textBoxmax.Text);

                if (max <= min || max <= (min + 2)) //�P�_��J����O�_���T
                {
                    if (max < min)  //��̤j��<�̤p��
                    {
                        MessageBox.Show("��J���~! �̤j�Ȥ��i�C��̤p��");
                    }
                    else if (max == min) //��̤j��=�̤p��
                    {
                        MessageBox.Show("��J���~! �̤j�ȻP�̤p��1���i�۵�");
                    }
                    else if (max <= (min + 2))  //��̤j��<=�̤p��+2
                    {
                        MessageBox.Show("��J���~! �̤j�ȻP�̤p�Ȭۮt�ݤj��2");
                    }

                    textBoxmin.Clear();
                    textBoxmax.Clear();
                    textBoxmin.Focus();
                }
                else if (max > min) //�ŦX���ͱ���
                {
                    Lbmin.Text = min.ToString();
                    Lbmax.Text = max.ToString();

                    answer = number.Next(min + 1, max);  //���ͱK�X
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

        private void BtnOK_Click(object sender, EventArgs e) // OK��
        {
            i = 0;
            string t = textBox.Text;
            bool result = int.TryParse(t, out i);  //�P�_��J���ȬO�_�������

            if (result == false)
            {
                MessageBox.Show("��J���~!�п�J�����!");
                textBox.Clear();
                textBox.Focus();
            }
            else if (result == true && RadioBtnFriend.Checked == true)  // �u�H�Ҧ�
            {
                if (int.Parse(textBox.Text) >= int.Parse(Lbmax.Text) || int.Parse(textBox.Text) <= int.Parse(Lbmin.Text)) //������J�d��
                {
                    MessageBox.Show("��J���~!�п�J�d�򤺪���!");
                    textBox.Clear();
                    textBox.Focus();
                }
                else if (int.Parse(textBox.Text) < answer)  // ���J���Ȥp��K�X
                {
                    Lbmin.Text = textBox.Text;
                    MessageBox.Show(Lbmin.Text + "~" + Lbmax.Text);
                    textBox.Clear();
                    textBox.Focus();
                }
                else if (int.Parse(textBox.Text) > answer) // ���J���Ȥj��K�X
                {
                    Lbmax.Text = textBox.Text;
                    MessageBox.Show(Lbmin.Text + "~" + Lbmax.Text);
                    textBox.Clear();
                    textBox.Focus();
                }
                else if (int.Parse(textBox.Text) == answer) // ���J���ȵ���K�X
                {
                    MessageBox.Show("Congratulations! ���״N�O " + answer);
                    BtnReset.PerformClick();
                }
            }
            else if (result == true && RadioBtnFriend.Checked == false) // �q����ԼҦ�
            {
                if (int.Parse(textBox.Text) >= int.Parse(Lbmax.Text) || int.Parse(textBox.Text) <= int.Parse(Lbmin.Text)) //������J�d��
                {
                    MessageBox.Show("��J���~!�п�J�d�򤺪���!");
                    textBox.Text = "0";
                    textBox.Focus();
                }
                // �u�H�@��
                else if (int.Parse(textBox.Text) < answer)  // ���J���Ȥp��K�X
                {
                    Lbmin.Text = textBox.Text;
                    MessageBox.Show(Lbmin.Text + "~" + Lbmax.Text);
                    MessageBox.Show("���q���o~");
                    textBox.Focus();
                }
                else if (int.Parse(textBox.Text) > answer) // ���J���Ȥj��K�X
                {
                    Lbmax.Text = textBox.Text;
                    MessageBox.Show(Lbmin.Text + "~" + Lbmax.Text);
                    MessageBox.Show("���q���o~");

                    textBox.Focus();
                }
                else if (int.Parse(textBox.Text) == answer) // ���J���ȵ���K�X
                {
                    MessageBox.Show("Congratulations! \n\r���״N�O " + answer);
                    BtnReset.PerformClick();
                    textBox.Text = "0";
                }
                if (int.Parse(textBox.Text) != answer && int.Parse(textBox.Text) != 0)
                {
                    int cmpnumber = number.Next((int.Parse(Lbmin.Text)) + 1, int.Parse(Lbmax.Text));

                    //�q���@��

                    if (cmpnumber < answer)  // ���J���Ȥp��K�X
                    {
                        Lbmin.Text = cmpnumber.ToString();
                        MessageBox.Show("�q���q:" + cmpnumber);
                        MessageBox.Show(Lbmin.Text + "~" + Lbmax.Text);
                        textBox.Clear();
                        textBox.Focus();
                    }
                    else if (cmpnumber > answer) // ���J���Ȥj��K�X
                    {
                        Lbmax.Text = cmpnumber.ToString();
                        MessageBox.Show("�q���q:" + cmpnumber);
                        MessageBox.Show(Lbmin.Text + "~" + Lbmax.Text);
                        textBox.Clear();
                        textBox.Focus();
                    }
                    else if (cmpnumber == answer) // ���J���ȵ���K�X
                    {
                        MessageBox.Show("�q���q:" + answer);
                        MessageBox.Show("�q�����!\n\r���״N�O " + answer);
                        BtnReset.PerformClick();
                    }
                }
                textBox.Clear();
                textBox.Focus();
            }
        }

        private void BtnReset_Click(object sender, EventArgs e) //���s�}�l
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
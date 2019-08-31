# aaa

using System;
using System.Collections.Generic;
using System.ComponentModel;
using System.Data;
using System.Drawing;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using System.Windows.Forms;
using System.Data.Sql;
using System.Data.SqlClient;
using System.Data.SqlTypes;

namespace ac
{
    public partial class zyadkrdn_shwen : Form
    {
        DataSet ds = new DataSet();
        SqlDataAdapter d;
        SqlCommandBuilder z;

        SqlConnection con = log.cn;
        public zyadkrdn_shwen()
        {
            InitializeComponent();
        }
        private void Load_shwen()
        {
            if (con.State == ConnectionState.Open)
            {
                con.Close();
            }

            try
            {
                string strCom = "Select naw from shwenakan";



                con.Open();

                SqlCommand Comm = new SqlCommand(strCom, con);
                SqlDataReader dr = Comm.ExecuteReader();

                comboBox1.Items.Clear();

                comboBox1.Items.Add("شوێنی دیاری کراو هەڵبژێرە");

                while (dr.Read())
                {
                    comboBox1.Items.Add(dr[0]);
                }

                comboBox1.SelectedIndex = 0;

                Comm.Dispose();
                con.Close();



            }
            catch (Exception ex)
            {
                MessageBox.Show(ex.Message, "SMS", MessageBoxButtons.OK, MessageBoxIcon.Error);
            }
        }


        private void Button5_Click(object sender, EventArgs e)
        {
            con.Open();
            Form2 a = new Form2();
            a.Show();
            this.Hide();
            con.Close();
        }

        private void Zyadkrdn_shwen_Load(object sender, EventArgs e)
        {
            Load_shwen();

            button1.Visible = false;
            textBox1.Visible = false;
            label4.Visible = false;
            dataGridView1.RowTemplate.Height = 30;
            if (textBox1.Text == "")
            {
                textBox1.Text = "شوێنە تازەکە بنووسە ";
                textBox1.ForeColor = Color.Silver;
            }
        }

        private void Button1_Click(object sender, EventArgs e)
        {
            con.Open();
            try
            {


                SqlCommand Comm = new SqlCommand("insert into shwenakan values(N'" + textBox1.Text + "')", con);
                Comm.ExecuteNonQuery();
                MessageBox.Show("ناوی شوێنە بە سەرکەوتویی زیاد کرا ");

            }
            catch (Exception ex)
            {
                MessageBox.Show(ex.Message, "سیستمی سوپەر ئارمۆر ", MessageBoxButtons.OK, MessageBoxIcon.Error);
            }
            con.Close();
            Load_shwen();
        }

        private void Button2_Click(object sender, EventArgs e)
        {
            button1.Visible = true;
            textBox1.Visible = true;
            label4.Visible = true;
        }

        private void Button3_Click(object sender, EventArgs e)
        {
            dataGridView1.Visible = true;
            con.Open();
            try
            {

                if (dataGridView1.Rows.Count == 0 && dataGridView1.Rows != null)
                {
                    d = new SqlDataAdapter("select shw_id as'ژمارە', naw as'بەش', from shwenakan where Department.C_id = (select c_id from College where c_name=N'" + comboBox1.SelectedItem.ToString() + "')", con);
                    d.Fill(ds);

                    dataGridView1.DataSource = ds.Tables[0];
                    // this.dataGridView1.Columns[3].Visible = false;    تەواو نەبووە ئەمە زۆری ماوە
                    //this.dataGridView1.Columns[4].Visible = false;
                }
            }
             
            catch (Exception ex)
            {
                MessageBox.Show(ex.Message, "Darmanxana", MessageBoxButtons.OK, MessageBoxIcon.Error);
            }
             con.Close();




        }

        private void Button4_Click(object sender, EventArgs e)
        {
            z = new SqlCommandBuilder(d);
            d.Update(ds);
        }

        private void TextBox1_TextChanged(object sender, EventArgs e)
        {
            if (textBox1.Text == "شوێنە تازەکە بنووسە ")
            {
                textBox1.Text = " ";
                textBox1.ForeColor = Color.Black;
            }
        }

        private void ComboBox1_SelectedIndexChanged(object sender, EventArgs e)
        {
            if (this.dataGridView1.DataSource != null)
            {
                this.dataGridView1.DataSource = null;
                ds.Clear();
            }
            else
            {
                this.dataGridView1.Rows.Clear();
                ds.Clear();
            }
        }

        private void ComboBox1_Click(object sender, EventArgs e)
        {
            con.Open();
            textBox1.Visible = false;
            button1.Visible = false;
            con.Close();
        }

        private void Button6_Click(object sender, EventArgs e)
        {
            con.Open();
            MessageBox.Show("دڵنیای دەتەوێ پڕۆگرامەکە دابخەیت");
            System.Windows.Forms.Application.Exit();
            con.Close();
        }
    }


}

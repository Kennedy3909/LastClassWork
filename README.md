# LastClassWork
using System.Collections.Generic;
using System.ComponentModel;
using System.Data;
using System.Drawing;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using System.Windows.Forms;

namespace ClassExample7
{
    public partial class ClasssExample7 : Form
    {
        public ClasssExample7()
        {
            InitializeComponent();
        }
        public static string InputBox(string prompt, string title, string defaultValue)
        {
            InputBoxDialog ib = new InputBoxDialog();
            ib.FormPrompt = prompt;
            ib.FormCaption = title;
            ib.DefaultValue = defaultValue;
            ib.ShowDialog();
            string s = ib.InputResponse;
            ib.Close();
            return s;
        }

        const int ACCN = 90;
        const int ENG = 100;
        const int MTH1011 = 120;
        const int MTH12 = 150;
        const int SCI1011 = 100;
        const int SCI12 = 120;
        const double CashDisc = 0.05;
        const double HrsDisc = 0.02;
        const double VAT = 0.1;

        double AccAmountOwing;
        double AccVAT;
        double AccDiscAmt;
        string strSub = "";


        private void Form1_Load(object sender, EventArgs e)
        {

        }

        private void btnProcess_Click(object sender, EventArgs e)
        {
            double dblAmtOwing = 0;//declaration
            double dblVATAmt = 0;
            double dblCashDisc = 0;
            double dblHrsDisc = 0;
            double dblDiscAmt = 0;
            double dblTotalDisc = 0;
            string strSub = "";
            int intHrs = Convert.ToInt16(NumHrs.Value);
            //string strPhone = (txtPhoneNumber.Text);
            bool blnValidSubject = true;
            bool blnValidHours = true;
            bool blnValidInput = true;

            //Validate method called
            blnValidInput = ValidateInput(blnValidInput, intHrs);
            if (blnValidInput == true)
            {
                //subject Method
                strSub = GetSubject(strSub);
                while (strSub != "ZZZ")
                {
                    blnValidSubject = true;
                    blnValidSubject = ValidateSubject(blnValidSubject, strSub);

                    if (blnValidSubject == true)
                    {
                        intHrs = GetIntHrs(intHrs);
                        blnValidHours = true;
                        blnValidHours = ValidateHours(blnValidHours, intHrs);

                        if (blnValidHours == true)
                        {
                            dblAmtOwing = CalcAmtOwing(strSub, dblAmtOwing ,intHrs);

                        }
                    }
                    strSub = GetSubject(strSub);
                }//end of while loop

                //Hours dicount method called

                dblAmtOwing = CalcAmtOwing(strSub, dblAmtOwing, intHrs);
                dblHrsDisc = CalcHrsDisc(dblHrsDisc, dblAmtOwing, intHrs);
                dblAmtOwing -= dblHrsDisc;

                dblCashDisc = CalcCashDisc(dblCashDisc, dblAmtOwing, intHrs);
                dblAmtOwing -= dblCashDisc;

                dblTotalDisc = dblHrsDisc + dblCashDisc;

                dblVATAmt = dblAmtOwing * VAT;
                dblAmtOwing = dblAmtOwing + dblVATAmt;

                AccAmtOwing(dblAmtOwing, dblTotalDisc, dblVATAmt);

                DisplayOutput(dblAmtOwing, dblTotalDisc, dblVATAmt);


            }//end of if statement




        }//end of btn process

        private bool ValidateInput(bool blnValidInput, int intHrs)
        {
            if (txtPhone.Text == "")
            {
                MessageBox.Show("Please enter your phone", "ERROR");
                blnValidInput = false;
            }
            if (!radp1011.Checked && !rad12.Checked)
            {
                MessageBox.Show("Please enter your grade", "ERROR");
                blnValidInput = false;
            }
            return blnValidInput;
        }//End of Validate method



        private double CalcAmtOwing(string strSub, double dblAmtOwing, int intHrs)
        {
            if (strSub == "ACCN")
            {
                dblAmtOwing = ACCN * intHrs;
            }
            else
               if (strSub == "ENG")
            {
                dblAmtOwing = ENG * intHrs;
            }
            else
               if (strSub == "MTH" && radp1011.Checked == true)
            {
                dblAmtOwing = MTH1011 * intHrs;
            }
            else
               if (strSub == "MTH" && rad12.Checked == true)
            {
                dblAmtOwing = MTH12 + intHrs;
            }
            else
                   if (strSub == "SCI" && radp1011.Checked == true)
            {
                dblAmtOwing = SCI1011 * intHrs;
            }
            else
                   if (strSub == "SCI" && rad12.Checked == true)
            {
                dblAmtOwing = SCI12 * intHrs;
            }
            return dblAmtOwing;

        }
        private double CalcHrsDisc(double dblHrsDisc, double dblAmtOwing, int intHrs)
        {
            if (intHrs > 3)
            {
                dblHrsDisc = dblAmtOwing * HrsDisc;
                dblAmtOwing = dblAmtOwing - dblHrsDisc;
            }
            return dblHrsDisc;

        }
        private double CalcCashDisc(double dblCashDisc, double dblAmtOwing, int intHrs)
        {
            if (chkCash.Checked == true)
            {
                dblCashDisc = dblAmtOwing * CashDisc;
                dblAmtOwing = dblAmtOwing - dblCashDisc;
            }
            return dblCashDisc;
        }
        private void AccAmtOwing(double dblAmtOwing, double dblTotalDisc, double dblVATAmt)
        {
            AccAmountOwing = (AccAmountOwing + dblAmtOwing);
            AccDiscAmt = (AccDiscAmt + dblTotalDisc);
            AccVAT = (AccVAT + dblVATAmt);

        }
        private void DisplayOutput(double dblAmtOwing, double dblTotalDisc, double dblVATAmt)
        {
            lblAmtOwing.Text = dblAmtOwing.ToString("C2");
            lblVAT.Text = dblVATAmt.ToString("C2");
            lblDiscAmt.Text = dblTotalDisc.ToString("C2");

            lblAccDisc.Text = AccDiscAmt.ToString("C2");
            lblAccAmtOwing.Text = AccAmountOwing.ToString("C2");
            lblAccVat.Text = AccVAT.ToString("C2");

        }
        private string GetSubject(string strSub)
        {
            strSub = InputBox("Please enter subject", "", "");
            return strSub;
        }
        private bool ValidateSubject(bool blnValidSubject, string strSub)
        {
            if (strSub != "ACCN" || strSub != "ENG" || strSub != "SCI")
            {
                MessageBox.Show("Please enter a subject", "ERROR");
                blnValidSubject = false;
            }
            return blnValidSubject;
        }
        private int GetIntHrs(int intHrs)
        {
            intHrs = Convert.ToInt32(InputBox("Please enter valid hours", "", ""));
            return intHrs;
        }
        private bool ValidateHours(bool blnValidHours, int intHrs)
        {
            if (intHrs < 1)
            {
                MessageBox.Show("Please enter valid hours", "ERROR");
                blnValidHours = false;
            }
            return blnValidHours;
        }

        private void btnClear_Click(object sender, EventArgs e)
        {
            lblAmtOwing.Text = "";
            lblDiscAmt.Text = "";
            lblVAT.Text = "";

            txtPhone.Text = "";
            chkCash.Checked=false;
            radp1011.Checked = false;
            rad12.Checked = false;
        }

        private void btnExit_Click(object sender, EventArgs e)
        {
            Application.Exit();
        }
    }
}

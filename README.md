usingSystem;

usingSystem.Collections.Generic;

usingSystem.Linq;

usingSystem.Web;

usingSystem.Web.UI;

usingSystem.Web.UI.WebControls;

usingSystem.Data.SqlClient;

publicpartialclassRegister:System.Web.UI.Page

{

intcount;

SqlConnectioncon=newSqlConnection("DataSource=.;InitialCatalog=hallbooking;Integrated

Security=True");

protectedvoidPage_Load(objectsender,EventArgse)

{

rid();

}

protectedvoidButton1_Click(objectsender,EventArgse)

{

con.Close();

con.Open();

SqlCommandcmd=newSqlCommand("insertintoregistervalues('"+TextBox1.Text+"','"+

TextBox2.Text+ "','"+DropDownList1.Text+"','"+ TextBox3.Text+ "','"+ TextBox4.Text+ "','"+

TextBox5.Text+ "','"+DropDownList2.Text+"','"+DropDownList3.Text+"','"+ TextBox6.Text+ "','"+

TextBox7.Text+"','"+TextBox8.Text+"')",con);

cmd.ExecuteNonQuery();

Response.Write("<script>alert('RegisteredSuccessfully')</script>");

con.Close();

}

privatevoidrid()

{

con.Close();

con.Open();

SqlCommandcmd=newSqlCommand("selectcount(*)fromregister",con);

cmd.ExecuteNonQuery();

count=Convert.ToInt16(cmd.ExecuteScalar())+1;

TextBox1.Text=count.ToString();

con.Close();

}

}

usingSystem;

usingSystem.Collections.Generic;

usingSystem.Linq;

usingSystem.Web;

usingSystem.Web.UI;

usingSystem.Web.UI.WebControls;

usingSystem.Data.SqlClient;

publicpartialclassLogin:System.Web.UI.Page

{

intcount;

SqlConnectioncon=newSqlConnection("DataSource=.;InitialCatalog=hallbooking;Integrated

Security=True");

protectedvoidPage_Load(objectsender,EventArgse)

{

}

protectedvoidButton1_Click(objectsender,EventArgse)
{

if(TextBox1.Text=="Admin"&&TextBox2.Text=="Admin")

{

Session["username"]=TextBox1.Text;

Response.Redirect("AdminHome.aspx");

}

else

{

con.Close();

con.Open();

SqlCommandcmd=newSqlCommand("selectusername,passwordfrom registerwhere

username='"+TextBox1.Text+"'andpassword='"+TextBox2.Text+"'",con);

SqlDataReaderdr=cmd.ExecuteReader();

if(dr.Read())

{

Session["username"]=TextBox1.Text;

rid();

Response.Redirect("UserHome.aspx");

}

else

{

}

}

}

protectedvoidButton2_Click(objectsender,EventArgse)

{

TextBox1.Text="";

TextBox2.Text="";

}

privatevoidrid()

{

con.Close();

con.Open();

SqlCommandcmd=newSqlCommand("selectregisterid,registernamefrom registerwhere

username='"+TextBox1.Text+"'",con);

SqlDataReaderdr=cmd.ExecuteReader();

if(dr.Read())

{

Session["rid"]=dr[0].ToString();

Session["rname"]=dr[0].ToString();

}

dr.Close();

con.Close();

}

}

usingSystem;

usingSystem.Collections.Generic;

usingSystem.Linq;

usingSystem.Web;

usingSystem.Web.UI;

usingSystem.Web.UI.WebControls;

usingSystem.Data.SqlClient;

publicpartialclassAddEvent:System.Web.UI.Page

{

intcount;

SqlConnectioncon=newSqlConnection("DataSource=.;InitialCatalog=hallbooking;Integrated Security=True");

protectedvoidPage_Load(objectsender,EventArgse)
{

Label1.Text=Session["rid"].ToString();

TextBox1.Text=System.DateTime.Now.ToShortDateString();

if(IsPostBack!=true)

{

rid();

hid();

}

}

protectedvoidButton1_Click(objectsender,EventArgse)

{

con.Close();

con.Open();

SqlCommandcmd=newSqlCommand("insertintobookingvalues('"+TextBox1.Text+"','"+

TextBox2.Text+ "','"+DropDownList1.Text+"','"+ TextBox3.Text+ "','"+ TextBox4.Text+ "','"+

TextBox5.Text+"','"+TextBox6.Text+"','"+DropDownList3.Text+"','"+TextBox7.Text+"')",con);

cmd.ExecuteNonQuery();

Response.Write("<script>alert('HallBookingAddedSuccessfully')</script>");

con.Close();

}

privatevoidrid()

{

con.Close();

con.Open();

SqlCommand cmd = new SqlCommand("select registerid from register where

registerid='"+Label1.Text+"'",con);

SqlDataReaderdr=cmd.ExecuteReader();

if(dr.Read())

{

DropDownList1.Items.Add(dr[0].ToString());

}

con.Close();

}

privatevoidhid()

{

con.Close();

con.Open();

SqlCommandcmd=newSqlCommand("selecthallidfromaddhall",con);

SqlDataReaderdr=cmd.ExecuteReader();

while(dr.Read())

{

DropDownList3.Items.Add(dr[0].ToString());

}

con.Close();

}

protectedvoidDropDownList1_SelectedIndexChanged(objectsender,EventArgse)

{

con.Close();

con.Open();

SqlCommand cmd = new SqlCommand("select registername from register where registerid='"+DropDownList1.Text+"'",con);

SqlDataReaderdr=cmd.ExecuteReader();
if(dr.Read())
{

TextBox3.Text=dr[0].ToString();

}

}

protectedvoidDropDownList3_SelectedIndexChanged(objectsender,EventArgse)

{

con.Close();

con.Open();

SqlCommandcmd=new SqlCommand("selectlocationfrom addhallwherehallid='"+

DropDownList3.Text+"'",con);

SqlDataReaderdr=cmd.ExecuteReader();

if(dr.Read())

{

TextBox7.Text=dr[0].ToString();

}

}

protectedvoidButton2_Click(objectsender,EventArgse)

{

}

}

usingSystem;

usingSystem.Collections.Generic;

usingSystem.Linq;

usingSystem.Web;

usingSystem.Web.UI;

usingSystem.Web.UI.WebControls;

usingSystem.Data.SqlClient;

publicpartialclassAddHall:System.Web.UI.Page

{

intcount;

SqlConnectioncon=newSqlConnection("DataSource=.;InitialCatalog=hallbooking;Integrated

Security=True");

protectedvoidPage_Load(objectsender,EventArgse)

{

hid();

}

protectedvoidButton1_Click(objectsender,EventArgse)

{

con.Close();

con.Open();

stringimgName=FileUpload1.FileName;

stringimgPath="halls/"+imgName;

intimgSize=FileUpload1.PostedFile.ContentLength;

FileUpload1.SaveAs(Server.MapPath(imgPath));

Image2.ImageUrl="~/"+imgPath;

SqlCommandcmd=newSqlCommand("insertintoaddhallvalues('"+TextBox1.Text+"','"+

TextBox2.Text+ "','"+ DropDownList1.Text+ "','"+ TextBox3.Text+ "','"+ TextBox4.Text+ "','"+

TextBox5.Text+"','"+TextBox6.Text+"','"+TextBox7.Text+"','"+imgName+"','"+imgPath+"','"+

imgSize+"')",con);

cmd.ExecuteNonQuery();

Response.Write("<script>alert('NewHallAddedSuccessfully')</script>");
upload();

con.Close();

}

protectedvoidButton2_Click(objectsender,EventArgse)

{

TextBox2.Text="";

TextBox3.Text="";
TextBox4.Text="";

TextBox5.Text="";

TextBox6.Text="";

DropDownList1.Text="";

}

privatevoidhid()

{

con.Close();

con.Open();

SqlCommandcmd=newSqlCommand("selectcount(*)fromaddhall",con);

cmd.ExecuteNonQuery();

count=Convert.ToInt16(cmd.ExecuteScalar())+1;

TextBox1.Text=count.ToString();

con.Close();

}

privatevoidupload()

{

con.Close();

con.Open();

SqlCommand cmd = new SqlCommand("select path from addhallwhere hallid='"+

TextBox1.Text+"'",con);

SqlDataReaderdr=cmd.ExecuteReader();

if(dr.Read())

{

Image2.ImageUrl=(dr[0].ToString());

}

else

{

}

}

}

usingSystem;

usingSystem.Collections.Generic;

usingSystem.Linq;

usingSystem.Web;

usingSystem.Web.UI;

usingSystem.Web.UI.WebControls;

usingSystem.Data.SqlClient;

publicpartialclassAddApproval:System.Web.UI.Page

{

intcount;

SqlConnectioncon=newSqlConnection("DataSource=.;InitialCatalog=hallbooking;Integrated

Security=True");

protectedvoidPage_Load(objectsender,EventArgse)

{

TextBox1.Text=System.DateTime.Now.ToShortDateString();

aid();

if(IsPostBack!=true)

{

bid();

}

}

protectedvoidButton1_Click(objectsender,EventArgse)

{

con.Close();

con.Open();

SqlCommandcmd=newSqlCommand("insertintoaddapprovalvalues('"+TextBox1.Text+

"','"+TextBox2.Text+"','"+DropDownList1.Text +"','"+TextBox3.Text+"','"+TextBox4.Text+"','"+

TextBox5.Text+"','"+TextBox6.Text+"','"+TextBox7.Text+"','"+DropDownList3.Text+"','"+Label1.Text+"')",con);
cmd.ExecuteNonQuery();

Response.Write("<script>alert('ApprovalAddedSuccessfully')</script>");

con.Close();

}

privatevoidbid()

{

con.Close();

con.Open();

SqlCommandcmd=newSqlCommand("selectbookingidfrombooking",con);

SqlDataReaderdr=cmd.ExecuteReader();

while(dr.Read())

{

DropDownList1.Items.Add(dr[0].ToString());

}

con.Close();

}

protectedvoidDropDownList1_SelectedIndexChanged(objectsender,EventArgse)

{

con.Close();

con.Open();

SqlCommand cmd = new SqlCommand("select

hallid,eventname,bookingdate,totalcapacity,registername,registerid from booking wherebookingid='"+

DropDownList1.Text+"'",con);

SqlDataReaderdr=cmd.ExecuteReader();

if(dr.Read())

{

TextBox3.Text=dr[0].ToString();

TextBox4.Text=dr[1].ToString();

TextBox5.Text=dr[2].ToString();

TextBox6.Text=dr[3].ToString();

TextBox7.Text=dr[4].ToString();

Label1.Text=dr[5].ToString();

}

}

privatevoidaid()

{

con.Close();

con.Open();

SqlCommandcmd=newSqlCommand("selectcount(*)fromaddapproval",con);

cmd.ExecuteNonQuery();

count=Convert.ToInt16(cmd.ExecuteScalar())+1;

TextBox2.Text=count.ToString();

con.Close();

}

protectedvoidButton2_Click(objectsender,EventArgse)

{

}

}

usingSystem;

usingSystem.Collections.Generic;

usingSystem.Linq;

usingSystem.Web;

usingSystem.Web.UI;

usingSystem.Web.UI.WebControls;
usingSystem.Data.SqlClient;
publicpartialclassAddFeedback:System.Web.UI.Page

{

intcount;

SqlConnectioncon=newSqlConnection("DataSource=.;InitialCatalog=hallbooking;Integrated

Security=True");

protectedvoidPage_Load(objectsender,EventArgse)

{

if(IsPostBack!=true)

{

rid();

}

}

protectedvoidButton1_Click(objectsender,EventArgse)

{

con.Close();

con.Open();

SqlCommand cmd = new SqlCommand("insertinto feedback values('"+TextBox1.Text

+"','"+TextBox2.Text +"','"+DropDownList1.Text +"','"+TextBox3.Text +"','"+DropDownList2.Text

+"','"+TextBox4.Text+"')",con);

cmd.ExecuteNonQuery();

Response.Write("<script>alert('FeedbackAddedSuccessfully')</script>");

con.Close();

}

privatevoidrid()

{

con.Close();

con.Open();

SqlCommandcmd=newSqlCommand("selectregisteridfromregister",con);

SqlDataReaderdr=cmd.ExecuteReader();

while(dr.Read())

{

DropDownList1.Items.Add(dr[0].ToString());

}

con.Close();

}

protectedvoidButton2_Click(objectsender,EventArgse)

{

}

protectedvoidDropDownList1_SelectedIndexChanged(objectsender,EventArgse){

}

}

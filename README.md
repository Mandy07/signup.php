<html>

<head><title>Restaurant</title>

<link rel="stylesheet" href="css/style1.css" type="text/css" media="screen" />

<script type="application/javascript">

  function num(evt)
      {
         var charCode = (evt.which) ? evt.which : event.keyCode
         if (charCode > 31 && (charCode < 48 || charCode > 57))
            return false;

         return true;
      }

</script>

</head>

<body>


<form method = "post" action = "<?php $_SERVER['PHP_SELF']; ?>"


<div id="maincontainer">

<div id="header">
REGISTRATION FORM
</div>

<div id="body">

<br>
<div id="reg-outer">
<br>
<div class="reg-inner">

<fieldset>

<legend>
<font color="green">Personal information:</font>
</legend>

<BR>
<font color="red">*</font>First Name <INPUT TYPE="TEXT" NAME="fname" size="30" class="input_field"  value="<?php echo getvalue('fname'); ?>" />
<br>

<BR>
<font color="red">*</font>last Name &nbsp;&nbsp;<INPUT TYPE="TEXT" NAME="lname"  size="30"  class="input_field" value="<?php echo getvalue('lname'); ?>" />
<br>

<br><font color="red">*</font>Gender 
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<SELECT NAME="gender" class="input_field" >
<OPTION VALUE="I am...">I am</OPTION>
<OPTION VALUE="MALE">MALE</OPTION>
<OPTION VALUE="FEMALE">FEMALE</OPTION>
</SELECT>
<br>


<BR>
<font color="red">*</font>Mobile +91&nbsp;<INPUT TYPE="mob" NAME="mobile" class="input_field" onkeypress="return num(event)" maxlength="11" size="30"  value="<?php echo getvalue('mobile'); ?>"/>
<br>


<div align="right" >

<br><font color="red">*</font>E-mail-id <INPUT TYPE="email" NAME="email" class="input_field" SIZE="30"  value="<?php echo getvalue('email'); ?>" /><br>

<br>
<font color="red">*</font>Password  <INPUT TYPE="Password" class="input_field" NAME="PASS" size="30" />
<BR>

<br>
<font color="red">*</font>Confirm Password  <INPUT TYPE="PASSWORD" class="input_field" NAME="PASS1" size="30" />
<BR>
<br><font color="red">*</font>Address <textarea name="add" rows="3" cols="33"  value="<?php echo getvalue('add'); ?>" > </textarea> <br>

</div>

</fieldset>

<br><center><INPUT TYPE="submit" VALUE="Submit" name="button" /></center>


</form>



<?php
$flag=0;
if(isset($_POST['button']))

{

$firstname=$_POST['fname'];
$lastname=$_POST['lname'];
$gender=$_POST['gender'];
$mobile=$_POST['mobile'];
$email=$_POST['email'];
$password=$_POST['PASS'];
$cpassword=$_POST['PASS1'];
$add=$_POST['add'];

$host="localhost";
$user="root";
$pass="";
$db="rest";


$connection = mysql_connect($host,$user,$pass) or die("unable to connect");

mysql_select_db($db) or die("unable to connect");

$query = "select * from register";

$result = mysql_query($query) or die("unable to connect");

mysql_select_db("$db", $connection);

if(($firstname==null) || ($lastname==null) || ($mobile==null) || ($email==null) || ($add==null) )
{
	?><B><center><FONT COLOR="red"><?php echo "*Field cannot remain empty.<br>";?></FONT></center></B><?php
	$flag=1;

}

else if(($password==null) || ($cpassword==null)) 
{
	?><B><center><FONT COLOR="red"><?php echo "*Field cannot remain empty.<br>";?></FONT></center></B><?php
	$flag=1;

}


else if($password!=$cpassword)
{
        ?><B><center><FONT COLOR="red"><?php echo "please Enter a correct password<br>";?></FONT></center></B><?php
	$flag=1;

} 





else
{

	while($row=mysql_fetch_row($result))
	{
		if($email == $row[5])
		{
			?><B><center><FONT COLOR="red"><?php echo "*This email id already taken try another.<br>";?></FONT></center></B><?php
			$flag=1;
		}
	}
}


if($flag==0)
{


mysql_query("INSERT INTO register(fname,lname,gender, mobile, email, password, confirmpassword,address) VALUES('$firstname','$lastname','$gender','$mobile','$email','$password','$cpassword','$add')");
mysql_query("INSERT INTO login (email,password) values ('$email','$password')");
?><META HTTP-EQUIV="Refresh" Content="0"; URL="index.html">;
<?php
}	
	
	

else
{
	?><B><center><FONT COLOR="red"><?php echo "*Registration failed. Correct your details.";?></FONT></center></B><?php


}



}


function getvalue($val)
{
if(isset($_POST[$val]))
{
return $_POST[$val];
}
else
{
return "";
}

}
?>

</div>
</div>
</div>

</body>
</html>


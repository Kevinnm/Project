# Project
Login



Bestand: connect.php



<?php

$username = "root";
$password = "root";
$dbname = "administratie";

$conn = new mysqli("localhost", $username, $password, $dbname);

if ($conn->connect_error) {
  die("Connection failed: " . $conn->connect_error);
} else {
  echo "Connection geslaagd!";
}

?>





Bestand: edit.php




<?php

include "connect.php";

if (isset($_POST['bewerken'])) {

  $voornaam = $_POST['voornaam'];
  $tussenvoegsel = $_POST['tussenvoegsel'];
  $achternaam = $_POST['achternaam'];
  $id = $_POST['id'];
  $inlognaam = $_POST['inlognaam'];
  $wachtwoord = md5($_POST['wachtwoord']);

  $sql = "UPDATE personeel SET voornaam = '$voornaam', tussenvoegsel = '$tussenvoegsel', achternaam = '$achternaam', inlognaam = '$inlognaam', wachtwoord = '$wachtwoord' WHERE id = $id";
  $result = $conn->query($sql);




}


$sql = "SELECT * FROM personeel";

$result = $conn->query($sql);
  $conn->close();

?>

<table border="2">
<tr>
  <th>id</th>
  <th>voornaam</th>
  <th>tussenvoegsel</th>
  <th>achternaam</th>
  <th>status</th>
  <th>inlognaam</th>
  <th>wachtwoord</th>
</tr>
<?php

while ($row = $result->fetch_assoc())
{
  if (isset($_GET['id'])) {
if ($_GET['id'] == $row['id']) {
?>
<form method="post">
<?php
    echo "<tr>";
    echo "<td><input type='text' name='id' value='".$row["id"]."'></td>";
    echo "<td><input type='text' name='voornaam' value='".$row['voornaam']. "'></td>";
    echo "<td><input type='text' name='tussenvoegsel' value='".$row['tussenvoegsel']. "'></td>";
    echo "<td><input type='text' name='achternaam' value='".$row['achternaam']. "'></td>";
    echo "<td><input type='text' name='status' value='".$row['status']. "'></td>";
    echo "<td><input type='text' name='inlognaam' value='".$row['inlognaam']. "'></td>";
    echo "<td><input type='password' name='wachtwoord' value='".$row['wachtwoord']. "'></td>";
    echo "<td> <input type='submit' name='bewerken' value='Bewerken'/></td>";
    echo "</tr>";
    ?>
</form>
  <?php
}
} else {

    echo "<tr>";
    echo "<td>" . $row['id'] ."</td>";
    echo "<td>" . $row['voornaam'] . "</td>";
    echo "<td>" . $row['tussenvoegsel'] . "</td>";
    echo "<td>" . $row['achternaam'] . "</td>";
    echo "<td>" . $row['status'] . "</td>";
    echo "<td>" . $row['inlognaam'] . "</td>";
    echo "<td>" . $row['wachtwoord'] . "</td>";
    echo "<td> <a href='edit.php?id=". $row['id'] ."'>Bewerken </td>";
    echo "</tr>";
}
}


?>

</table>



Bestand: index.php


<?php

session_start();


?>
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8">
    <title>Welkom</title>
  </head>
  <body>
    <h1>Hi WELKOM <?php echo $_SESSION['inlognaam'] ; ?></h1>
  </body>
</html>



Bestand: login-backup.php


<?php

include "connect.php";

$name= $_POST['inlognaam'];
$loginbutton= $_POST['login'];

if ($loginbutton){
if (!empty($name)) {
echo 'The name you entered is ' . $name;
}
else {
echo 'You did not enter a name. Please enter a name into this form field.';
}
}



?>

<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8">
    <title>Login</title>
  </head>
  <body>
    <form method="post">
      Login: <br>
        <tr>
        <td><input type='text' name='inlognaam' value='<?= $row["inlognaam"]?>'></td>
        <br>
        <br>

        Wachtwoord: <br>

        <td><input type='password' name='wachtwoord' value='<?= $row["wachtwoord"]?>'></td>
        <br>
        <br>
        <td> <input type='submit' name='login' value='Login'/></td>
        </tr>

    </form>
  </body>
</html>


Bestand: login.php


<?php

include "connect.php";
session_start();

// If form submitted, insert values into the database.
if (isset($_POST['inlognaam'])){
        // removes backslashes
	$inlognaam = stripslashes($_POST['inlognaam']);
        //escapes special characters in a string
	$inlognaam = mysqli_real_escape_string($conn,$inlognaam);
	$wachtwoord = stripslashes($_REQUEST['wachtwoord']);
	$wachtwoord = mysqli_real_escape_string($conn,$wachtwoord);
	//Checking is user existing in the database or not
        $query = "SELECT * FROM `personeel` WHERE inlognaam='$inlognaam'
and wachtwoord='".md5($wachtwoord)."'";
  echo "$query";
	$result = mysqli_query($conn,$query) or die(mysql_error());
	$rows = mysqli_num_rows($result);
        if($rows==1){
	    $_SESSION['inlognaam'] = $inlognaam;
            // Redirect user to index.php
	    header("Location: index.php");
         }else{
	echo "<div class='form'>
<h3>Username/password is incorrect.</h3>
<br/>Click here to <a href='index.php'>Login</a></div>";
	}
}else{}




?>

<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8">
    <title>Login</title>
  </head>
  <body>
    <form method="post">
      Login: <br>
        <tr>
        <td><input type='text' name='inlognaam' value='<?= $row["inlognaam"]?>'></td>
        <br>
        <br>

        Wachtwoord: <br>

        <td><input type='password' name='wachtwoord' value='<?= $row["wachtwoord"]?>'></td>
        <br>
        <br>
        <td> <input type='submit' name='login' value='Login'/></td>
        </tr>

    </form>
  </body>
</html>



Bestand: nieuw.php


<?php

include "connect.php";

if (isset($_POST['submit'])) {
  $voornaam = $_POST['voornaam'];
  $tussenvoegsel = $_POST['tussenvoegsel'];
  $achternaam = $_POST['achternaam'];
  $inlognaam = $_POST['inlognaam'];
  $wachtwoord = md5($_POST['wachtwoord']);

  $sql = "INSERT INTO personeel (voornaam, tussenvoegsel, achternaam, status, inlognaam, wachtwoord)
  VALUES ('$voornaam', '$tussenvoegsel', '$achternaam', 1, '$inlognaam', '$wachtwoord')";

  if ($conn->query($sql) === TRUE) {
    echo "Nieuw personeelslid toegevoegd";
  } else {
    echo "ERROR: " . $sql . "<br>" . $conn->error;
  }

$conn->close();

}

?>
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8">
    <title>Formulier</title>
  </head>
  <body>
    <form method="post">
      Voornaam: <br><input type="text" name="voornaam" required><br><br>
      Tussenvoegsel: <br><input type="text" name="tussenvoegsel"><br><br>
      Achternaam: <br><input type="text" name="achternaam" required><br><br>
      Inlognaam: <br><input type="text" name="inlognaam" required><br><br>
      Wachtwoord: <br><input type="password" name="wachtwoord" required><br><br>
      <input type="submit" name="submit">
    </form>
  </body>
</html>

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


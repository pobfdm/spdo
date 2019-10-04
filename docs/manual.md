# The sPDO manual

Spdo is a simple Php class that allows the development PDO in a simple and
fast.The class in question does not tend to hide the methods and the PDO
driver functions (called up using the same public facilities) rather adds
a few others to make the job faster.

## Methods
- **connect()** 	connects to the db
- **disconnect()** 	disconnects from db
- **execute($sql)** run a query
- **nrows()** 		returns the number of records of a select
- **read($n, $column)** reads a field from a select, $n is the line number, $column is the column name 	
- **lastInsertId()** 	it returns the last record id of the last inserted through an insert
- **GetDriverName()** 	returns a string with the name of the driver you are using (eg. "mysql")
- **addslashes($str)** 	quote correctly strings in insert and update according to the type of database. (Mysql, sqlite, postgresql)
- **isSelect($sql)** 	returns TRUE if the query is a select, false otherwise.
- **quote($sql)** 	returns a quoted string to be inserted into a query
- **countRecordFromTable($table)**	returns the number of records in a table

## Show the result of a select

```
<?php

include_once("lib/sPDO.php");

$mydb = new sPDO();

$mydb->connect();
if ( !$mydb->execute("select * from users ")) echo "problemi nella query<br>";

for ($i=1; $i<= $mydb->nrows(); $i++)
{
	echo $mydb->read($i, "user") ." - ". $mydb->read($i, "pass") . "<br>" ;
}
$mydb->disconnect();
?>

```
## Insert a record in a table

```
<?php

include_once("lib/sPDO.php");

$mydb = new sPDO();

$mydb->connect();
$sql="INSERT INTO  `users` ( `user` ,`pass` , `type` ) VALUES ( 'utente' , '365816905f5e9c148e20273719fe163d' , 1 );" ;
if ( !$mydb->execute($sql)) echo "problemi nella insert <br>";
$mydb->disconnect();

?>
```

## Delete a record

```
<?php

include_once("lib/sPDO.php");

$mydb = new sPDO();


$mydb->connect();
$sql="DELETE  from `users` WHERE  `id` >3;" ;
if ( !$mydb->execute($sql)) echo "problemi nella delete <br>";
$mydb->disconnect();

?>
```

## Edit a record

```
<?php

include_once("lib/sPDO.php");

$mydb = new sPDO();

$mydb->connect();
$sql="UPDATE  `users` SET  `user` =  'test0',
						   `pass` =  '098f6bcd4621d373cade4e832627b4f6'
				WHERE  `id` =3;";
if ( !$mydb->execute($sql)) echo "problemi nella update <br>";
$mydb->disconnect();

?>
```


## Direct use of PDO

```
<?php

$db = new sPDO();
$db->connect();

$sql =  " UPDATE itcms_pages set
		title = :title,
		body =  :body,
		menu =  :menu,				
		menuorder = :menuorder				
		where id = :id " ; 											

		$db->stmt = $db->dbh->prepare($sql);
		$db->stmt->bindParam(":title", $_POST["title"]);
		$db->stmt->bindParam(":body", $_POST["body"]);
		$db->stmt->bindParam(":menu", $menu);
		$db->stmt->bindParam(":menuorder", $_POST["menuorder"]);
		$db->stmt->bindParam(":id", $_POST["id"]);

if (!$db->stmt->execute()) echo 'Errore nella query: '. $sql;

$db->disconnect();

?>
```

## Some practical examples of use with sqlite

### Create a table

```


<?php
	$mydb= new sPDO();
	$mydb->DSN =  'sqlite:db.sqlite' ;
	$mydb->connect();
	$sql='CREATE TABLE if not exists "persone" (
						  "id" INTEGER NOT NULL ,
						  "cognome" varchar(1) ,
						  "nome" varchar(256) ,
						  "tel" varchar(256) ,
						  "email" varchar(256) ,
						  PRIMARY KEY ("id")
						);' ;
	if (!$mydb->execute($sql)) echo 'Problemi nella query';
	$mydb->disconnect();					
?>


```

### Select

```
<?php
	$mydb= new sPDO();
	$mydb->DSN =  'sqlite:db.sqlite' ;
	$mydb->connect();
	$sql='Select * from persone;' ;
	if (!$mydb->execute($sql)) echo 'Problemi nella query';

	for ($i=1; $i<= $mydb->nrows(); $i++)
	{
		echo $mydb->read($i, "cognome") ." - ".
				$mydb->read($i, "nome") ." - ".
				$mydb->read($i, "tel")." - ".
				$mydb->read($i, "email"). "<br>" ;
	}
	echo '<div style="margin-top:15px">Record totali nella tabella: '.$mydb->countRecordFromTable('persone').'</div>';
	$mydb->disconnect();

?>


```

### Insert

```


<?php
	$mydb= new sPDO();
	$mydb->DSN =  'sqlite:db.sqlite' ;
	$mydb->connect();
	$sql='INSERT INTO persone (id,cognome,nome,tel,email)VALUES (null,"Cane","Chester","834502","chester@email.it")' ;
	if (!$mydb->execute($sql)) echo 'Problemi nella query';
	$mydb->disconnect();					
?>



```

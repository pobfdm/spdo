#The sPDO manual

Spdo is a simple class that allows the development PDO in a simple and 
fast.The class in question does not tend to hide the methods and the PDO
driver functions (called up using the same public facilities) rather adds
a few others to make the job faster.

##Methods
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

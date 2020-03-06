# INSERT query using PDO
1. [INSERT query with positional placeholders](#1)
2. [INSERT query with named placeholders](#2)
3. [INSERTing multiple rows](#3)

First of all make sure you've got a properly configured PDO connection variable that needs in order to run SQL queries with PDO and to inform you of the possible errors.

In order to run an INSERT query with PDO just follow the steps below:

* create a correct SQL INSERT statement
* replace all actual values with placeholders
* prepare the resulting query
* execute the statement, sending all the actual values in the form of array.

<a name="1"></a>
## INSERT query with positional placeholders
As usual, positional placeholders are more concise and easier to use
```php
$sql = "INSERT INTO users (name, surname, sex) VALUES (?,?,?)";
$stmt= $pdo->prepare($sql);
$stmt->execute([$name, $surname, $sex]);
```
or you can chain `execute()` to `prepare()`:

```php
$sql = "INSERT INTO users (name, surname, sex) VALUES (?,?,?)";
$pdo->prepare($sql)->execute([$name, $surname, $sex]);
```
Important! You don't have to check the result of `execute()` (as it is often shown in low-quality tutorials). Such a condition will make no sense, as in case of error, a PDOException will be thrown and the script execution will be terminated, which means such a condition will never reach the `else` part.
Neither a `try ... catch` operator should be used, unless you have a specific scenario to handle the error, such as a transaction rollback shown below. Please see the article about error reporting for the details.

<a name="2"></a>
## INSERT query with named placeholders
In case you have a predefined array with values, or prefer named placeholders in general, the code would be

```php
$data = [
	'name' => $name,
	'surname' => $surname,
	'sex' => $sex,
];
$sql = "INSERT INTO users (name, surname, sex) VALUES (:name, :surname, :sex)";
$stmt= $pdo->prepare($sql);
$stmt->execute($data);
```
or you can chain `execute()` to `prepare()`:

```php
$sql = "INSERT INTO users (name, surname, sex) VALUES (?,?,?)";
$pdo->prepare($sql)->execute($data);
```

<a name="3"></a>
## INSERTing multiple rows
As it's explained in the main article, a once prepared statement could be executed multiple times, slightly reducing the overhead on the query parsing. So it makes sense to use this feature when we need to insert multiple rows into the same table. Just a couple notes before we begin:

* make sure that the emulation mode is turned off, as there will be no speed benefit otherwise, however small it is.
* it's a good idea to wrap our queries in a transaction. In some circumstances it will greatly speed up the inserts, and it makes sense overall, to make sure that either all data has been added or none.
So in the end our code would be like

```php
$data = [
	'John','Doe', 22,
	'Jane','Roe', 19,
];
$stmt = $pdo->prepare("INSERT INTO users (name, surname, age) VALUES (?,?,?)");
try {
	$pdo->beginTransaction();
		foreach ($data as $row)
	{
		$stmt->execute($row);
	}
	$pdo->commit();
}catch (Exception $e){
	$pdo->rollback();
	throw $e;
}
```
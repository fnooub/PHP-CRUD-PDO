# SELECT query with PDO
1. [SELECT query without parameters](#1)
	- [Getting a single row](#2)
	- [Selecting multiple rows](#3)
2. [SELECT query with parameters](#4)
	- [SELECT query with positional placeholders](#5)
	- [SELECT query with named placeholders](#6)
	- [Selecting multiple rows](#7)

There are several ways to run a SELECT query using PDO, that differ mainly by the presence of parameters, type of parameters, and the result type. I will show examples for the every case so you can choose one that suits you best.

Just make sure you've got a properly configured PDO connection variable that needs in order to run SQL queries with PDO and to inform you of the possible errors.

<a name="1"></a>
## SELECT query without parameters
If there are no variables going to be used in the query, we can use a conventional `query()` method instead of prepare and execute.

```php
// select all users
$stmt = $pdo->query("SELECT * FROM users");
```
This will give us an `$stmt` object that can be used to fetch the actual rows.

<a name="2"></a>
### Getting a single row
If a query is supposed to return just a single row, then you can just call `fetch()` method of the `$stmt` variable:

```php
// getting the last registered user
$stmt = $pdo->query("SELECT * FROM users ORDER BY id DESC LIMIT 1");
$user = $stmt->fetch();
```
Note that in PHP you can "chain" method calls, calling a method of the returned object already, like:

```php
$user = $pdo->query("SELECT * FROM users ORDER BY id DESC LIMIT 1")->fetch();
```
<a name="3"></a>
### Selecting multiple rows
There are two ways to fetch multiple rows returned by a query. The most traditional way is to use the `fetch()` method within a `while` loop:

```php
$stmt = $pdo->query("SELECT * FROM users");
while ($row = $stmt->fetch()) {
	echo $row['name']."<br />\n";
}
```
This method could be recommended if rows have to be processed one by one. For example, if such processing is the only action that needs to be taken, or if the data needs to be pre-processed somehow before use.

But the most preferred way to fetch multiple rows which would to be shown on a web-page is calling the great helper method called `fetchAll()`. It will put all the rows returned by a query into a PHP array, that later can be used to output the data using a template (which is considered much better than echoing the data right during the fetch process). So the code would be

```php
$data = $pdo->query("SELECT * FROM users")->fetchAll();
// and somewhere later:
foreach ($data as $row) {
	echo $row['name']."<br />\n";
}
```
<a name="4"></a>
## SELECT query with parameters
But most of time we have to use a variable or two in the query, and in such a case we should use a prepared statement (also called a parameterized query), first preparing a query with parameters (or placeholder marks) and then executing it, sending variables separately.

In PDO we can use both positional and named placeholders. For simple queries, personally I prefer positional placeholders, I find them less verbose, but it's entirely a matter of taste.

<a name="5"></a>
### SELECT query with positional placeholders
```php
// select a particular user by id
$stmt = $pdo->prepare("SELECT * FROM users WHERE id=?");
$stmt->execute([$id]); 
$user = $stmt->fetch();
```
<a name="6"></a>
### SELECT query with named placeholders
```php
// select a particular user by id
$stmt = $pdo->prepare("SELECT * FROM users WHERE id=:id");
$stmt->execute(['id' => $id]); 
$user = $stmt->fetch();
```
<a name="7"></a>
### Selecting multiple rows
Fetching multiple rows from a prepared query would be identical to that from a query without parameters already shown:
```php
$stmt = $pdo->query("SELECT * FROM users LIMIT ?, ?");
$stmt->execute([$limit, $offset]); 
while ($row = $stmt->fetch()) {
	echo $row['name']."<br />\n";
}
```
or
```php
$stmt = $pdo->prepare("SELECT * FROM users LIMIT :limit, :offset");
$stmt->execute(['limit' => $limit, 'offset' => $offset]); 
$data = $stmt->fetchAll();
// and somewhere later:
foreach ($data as $row) {
	echo $row['name']."<br />\n";
}
```
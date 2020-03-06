# UPDATE query using PDO
1. [UPDATE query with positional placeholders](#1)
2. [UPDATE query with named placeholders](#2)

First of all, make sure you've got a properly configured PDO connection variable that is needed in order to run SQL queries using PDO (and to inform you of the possible errors).

In order to run an UPDATE query with PDO just follow the steps below:

* create a correct SQL UPDATE statement
* replace all actual values with placeholders
* prepare the resulting query
* execute the statement, sending all the actual values to `execute()` in the form of array.

<a name="1"></a>
## UPDATE query with positional placeholders
As usual, positional placeholders are more concise and easier to use

```php
$sql = "UPDATE users SET name=?, surname=?, sex=? WHERE id=?";
$stmt= $pdo->prepare($sql);
$stmt->execute([$name, $surname, $sex, $id]);
```
or you can chain `execute()` to `prepare()`:
```php
$sql = "UPDATE users SET name=?, surname=?, sex=? WHERE id=?";
$pdo->prepare($sql)->execute([$name, $surname, $sex, $id]);
```
<a name="2"></a>
## UPDATE query with named placeholders
In case you have a predefined array with values, or prefer named placeholders in general, the code would be

```php
$data = [
	'name' => $name,
	'surname' => $surname,
	'sex' => $sex,
	'id' => $id,
];
$sql = "UPDATE users SET name=:name, surname=:surname, sex=:sex WHERE id=:id";
$stmt= $pdo->prepare($sql);
$stmt->execute($data);
```
or you can chain `execute()` to `prepare()`:

```php
$sql = "UPDATE users SET name=:name, surname=:surname, sex=:sex WHERE id=:id";
$pdo->prepare($sql)->execute($data);
```
Remember that you should't wrap every query into a `try..catch` statement. Instead, let a possible error to bubble up to either the built-in PHP's or your custom error handler.
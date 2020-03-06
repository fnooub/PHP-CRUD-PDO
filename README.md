# PHP-CURD-PDO

## kết nối
```php
$db = new PDO("mysql:host=localhost;dbname=myDB;charset=utf8", "username", "password");
```
* đặt thuộc tính
```php
$db->setAttribute(PDO::ATTR_ERRMODE, PDO::ERRMODE_EXCEPTION);
$db->setAttribute(PDO::ATTR_EMULATE_PREPARES, false);
$db->setAttribute(PDO::ATTR_DEFAULT_FETCH_MODE, PDO::FETCH_ASSOC);
...
```

## insert
```php
$sql = "INSERT INTO users (name, age, email) VALUES (:name, :age, :email)";
$stmt = $db->prepare($sql);
$stmt->execute(array(':name' => $name, ':email' => $email, ':age' => $age));
```
* id cuối insert
`echo $db->lastInsertId();`

## update
```php
$sql = "UPDATE users SET name=:name, age=:age, email=:email WHERE id=:id";
$stmt = $db->prepare($sql);
$stmt->execute(array(':id' => $id, ':name' => $name, ':email' => $email, ':age' => $age));
```
## select
* query
```php
$rows = $db->query("SELECT * FROM users")->fetchAll();
```
* prepare
```php
$sql = "SELECT * FROM users WHERE id=:id";
$stmt = $db->prepare($sql);
$stmt->execute(array(':id' => $id));
$row = $stmt->fetch(PDO::FETCH_ASSOC);
```
* `PDO::FETCH_ASSOC`
* `PDO::FETCH_OBJ`
* `...`

đếm `echo $stmt->rowCount();`

## delete
* query
```php
$db->query("DELETE FROM users WHERE id=$id");
```
* prepare
```php
$sql = "DELETE FROM users WHERE id=:id";
$stmt = $db->prepare($sql);
$stmt->execute(array(':id' => $id));
```
* TRUNCATE
```php
$db->query("TRUNCATE TABLE users");
```
---
## VÍ DỤ:
```php
<?php

$db = new PDO("mysql:host=localhost;dbname=myDB;charset=utf8", "root", "");

print_r(get_categories_by_post_id(1));

function get_categories_by_post_id($id)
{
	global $db;
	$query = "SELECT * FROM categories AS c JOIN posts_categories AS pc ON pc.category_id = c.id WHERE pc.post_id = :id";
	$stmt = $db->prepare($query);
	$stmt->execute(array(':id' => $id));
	if ($stmt->rowCount() > 0) {
		return $stmt->fetchAll(PDO::FETCH_ASSOC);
	}
	return false;
}
```
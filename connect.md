# KẾT NỐI PDO MYSQL
---

### kết nối đơn giản
``` php
$db = new PDO("mysql:host=localhost;dbname=myDB;charset=utf8", "username", "password");
```

### kết nối đặt thuộc tính
``` php
$db = new PDO("mysql:host=localhost;dbname=myDB;charset=utf8", "username", "password");
$db->setAttribute(PDO::ATTR_ERRMODE, PDO::ERRMODE_EXCEPTION);
$db->setAttribute(PDO::ATTR_EMULATE_PREPARES, false);
$db->setAttribute(PDO::ATTR_DEFAULT_FETCH_MODE, PDO::FETCH_ASSOC);
```

### kết nối cơ bản
``` php
$servername = "localhost";
$username = "username";
$password = "password";

try {
	$db = new PDO("mysql:host=$servername;dbname=myDB;charset=utf8", $username, $password);
	// set the PDO error mode to exception
	$db->setAttribute(PDO::ATTR_ERRMODE, PDO::ERRMODE_EXCEPTION);
	echo "Connected successfully";
}
catch(PDOException $e)
{
	echo "Connection failed: " . $e->getMessage();
}
```

### kết nối cơ bản
```php
$host = 'localhost';
$db   = 'myDB';
$user = 'username';
$pass = 'password';
$charset = 'utf8mb4';

$options = [
	\PDO::ATTR_ERRMODE            => \PDO::ERRMODE_EXCEPTION,
	\PDO::ATTR_DEFAULT_FETCH_MODE => \PDO::FETCH_ASSOC,
	\PDO::ATTR_EMULATE_PREPARES   => false,
];
$dsn = "mysql:host=$host;dbname=$db;charset=$charset";
try {
	$pdo = new \PDO($dsn, $user, $pass, $options);
} catch (\PDOException $e) {
	throw new \PDOException($e->getMessage(), (int)$e->getCode());
}
```

### ngắt kết nối
``` php
$db = null;
```
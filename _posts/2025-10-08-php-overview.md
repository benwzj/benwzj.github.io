---
layout: post
title: PHP Overview
date: 2025-10-08
category: Programming
tags: PHP
toc: 
  - name: What is PHP
  - name: PHP - Forms
  - name: PHP Session handling
  - name: Common functions
---

## What is PHP
PHP (Hypertext Preprocessor)

- Just like other script language, using variables, loop, etc.
- It runs on the server side and is embedded directly into HTML. Used to create dynamic, interactive websites.
- PHP powers many popular platforms like WordPress and Laravel.
- All php scripts start with `<?php and with ?>`
- PHP can be placed directly inside HTML

### Example
```php
<?php
$author = "John Smiths";
$message = "How are you?";
echo $author . " says " . $message;
?>
```

## PHP - Forms

- `<form method="post" action="process.php" >`
  - Method specifies how the data will be sent
  - Action specifies the file to send to. E.g. `process.php`
- Post method sends all contents of a form with basically hidden headers (not easily visible to users)
- Get method sends all form input in the URL requested using `name=value` pairs separated by ampersands (&)
  - E.g. `process.php?name=John&number=345`
  - Is visible in the URL shown in the browser
- All form values are placed into an array
- Assume a form contains one textbox called “lastname" and the form is
submitted using the post method, invoking `process.php`
- process.php could access the form data using:
  - `$_POST[‘lastname’]` or
  - Use `extract($_POST)` to imports form fields as variables
- If the form used the get method, the form data would be
available as:
  - `$_GET[‘lastname’]`

## PHP Session handling

Session handling is a way to make the data available across all pages of aweb application for a user.
A session creates a file in a temporary directory on the server where registered session variables and their values are stored.

### Using Seesion Steps
- The `session_start()` function is used to start a new session or, resume an existing one.
- Session variables are stored in associative array called `$_SESSION[]`.
  - `session_start() ;`
  - `$_SESSION['username'] = $username;`
  - `$_SESSION['total_amount'] = 2000;`
- Make use of `isset()` function to check if a session variable is already set or not. 


## Common functions

### empty()
The `empty()` function checks whether a variable is empty or not.
For exampel: `empty($_POST)`

### header("Location:myphp.php")
- The header() function sends a raw HTTP header to a client.
- The header location can be used to redirect a user to another page.
```php
header("Location:myphp.php")
header("Location: https://www.google.com/")
```

### isset()
- The function isset() is used to determine is a variable is set/declared
and is not null.
- This function returns true if variable exists and has any value other
than null.

```php
session_start();
if (!isset($_SESSION['username'])){
  header("Location:login.php");
}
```

### PHP Exceptions

```php
<?php
try {
throw new Exception("This is an exception");
} catch(Exception $e) {
echo $e->getMessage();
}
?>
```


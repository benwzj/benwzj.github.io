---
layout: post
title: PHP Overview
date: 2025-10-08
category: Programming
tags: PHP

---

PHP (Hypertext Preprocessor)

- Just like other script language, using variables, loop, etc.
- It runs on the server side and is embedded directly into HTML. Used to create dynamic, interactive websites.
- PHP powers many popular platforms like WordPress and Laravel.
- All php scripts start with `<?php and with ?>`
- PHP can be placed directly inside HTML

## PHP script - Example
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
  - Action specifies the file to send to. E.g. process.php
- Post method sends all contents of a form with basically hidden headers (not easily visible to users)
- Get method sends all form input in the URL requested using name=value pairs separated by ampersands (&)
  - E.g. process.php?name=John&number=345
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




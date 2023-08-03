
# A Symbolic open

This flow chart shows us that this program, takes an input file name, in several cases it exit display an error message, these cases are :

-   Invalid number of **ARGC**
-   Invalid **`open()`** / **`read()`**
-   Identical filename ("**token**")

![enter image description here](https://i.imgur.com/kh5v9KZ.png)


Suppose the flag is in the "**token**" file. This program has access to it, but it cannot **`open()`** it because of the **`strstr()`** condition. A very trivial solution would simply be to create a symbolic link on "**token**" in 
order to bypass it.

```bash
ln -s /home/user/level08/token /tmp/symb && ./level08 /tmp/symb
```

> Token : `quif5eloekouj29ke0vouxean`

> Flag : `25749xKZ8L7DkSCwJkT9dyv6f`

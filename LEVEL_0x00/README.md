# John file & Shift Cipher
This first level consists of finding a file whose owner is "**flag00"**, i was able to find this information 
in the related video of the project on intra.42.fr.

![enter image description here](https://i.imgur.com/9DiCT9C_d.webp?maxwidth=1520&fidelity=grand)

Below, i show you the command to execute in order to achieve it :

```bash
find / -user flag00 2> /dev/null

# find : find - search for files in a directory hierarchy
# / : recursive path
# -user flag00 : File is owned by user uname (numeric user ID allowed).
# 2> /dev/null : filter stderr output into /dev/null
```

The filtred output :
```
...
/usr/sbin/john
/rofs/usr/sbin/john
```

The token is in the file **`/usr/sbin/john`**, content : `cdiiddwpgswtgt`.
As you can see, the token doesn't work, that means it's not in its final form yet.
We can use tools like **dcode**, they have a cipher detector.

![enter image description here](https://i.imgur.com/CMNdN2N.png)

> Token : `nottoohardhere`

> Flag : `x24ti5gi3x0ol2eh4esiuxias`


# Load in EAX during Runtime
By running level13 we get this output :

    UID 2013 started us but we we expect 4242

The id 2013 corresponds to the level13, but i'm unable to find a user **4242**, let’s reverse the logic of this program.

![enter image description here](https://i.imgur.com/OX3RCRZ.png)

This program calls **`getuid()`**. If the return value is 4242, it gets us the flag. By examining the program with the **strings** command, we can locate a modified token. The **`ft_des()`** function contains the necessary 
instructions to convert it to a valid token.

![enter image description here](https://i.imgur.com/4nXp1sN.png)

GDB allows us to modify registers during the execution course, so we will modify **$eax** on 4242 before submitting this comparison :

```asm
0x804859a <main+14> cmp eax,0x1092
```

To do this, I use these steps, there are surely more effective.

`set $eax = 4242` before `0x804859a <main+14>             cmp    eax,0x1092`.

Then, just place yourself below ft_des to read on the registry that contains the token **$eax**.

> Flag : `2A31L79asukciNyi8uppkEuSx`

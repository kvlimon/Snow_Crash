# John the Ripper, pwd crack with cipher

Here, you can see something unusual with :

```bash
cat /etc/passwd
```

On the **flag01** user line, the password is clear, while on the other lines, it is hidden with a special X 
character. These passwords can be stored in the/etc/shadow file.

In order to understand the formatting of this file, we will take the usual line as an example :

![enter image description here](https://i.imgur.com/YjYq4qa.png)

 1. **Username** : The unique name of the user.
 2. **Password** : Historically, the password was stored in this field, but nowadays it is usually 
represented by an encrypted string (like a hash) for security reasons. A special character (for example, "x") 
can be used to indicate that the password is stored in the/etc/shadow file (a secure file accessible only by 
the system administrator).
 3. **UID** : A unique numeric identifier assigned to each user. This number is used internally by the system 
to identify the user.
 4. **GID** : The identifier of the main group to which the user belongs. The main group is usually used to 
determine the user’s file and directory access permissions.
 5. **Comments (User Info)** : Additional information about the user, such as their full name or description.
 6. **Home Directory** : The absolute path to the user’s home directory. This is where the user is when 
logging in.
 7. **Shell (Default shell)** : Le programme de shell qui sera utilisé lorsque l'utilisateur ouvrira une 
session. Cela détermine l'interface en ligne de commande avec laquelle l'utilisateur interagira.

Let’s assume that the password displayed is a hashed passphrase like the passwords in the **`/etc/shadow`** 
file.

In this case we have tools like John the ripper.

John is able to attack hashed passwords with different hash functions including the following algorithms : 
***MD5***, ***Blowfish***, ***Kerberos***, ***AFS***, and ***LM hashes from Windows NT/2000/XP/2003***.

To be able to use this tool instantly, we will run it in a Docker container.

 1. Create a **`/JTR`** directory
 2. Put the passphrase hashed in a **`toCrack.txt`** file.
 3. Create the **`Dockerfile`**.
 4. Launch your container, and now you can use John, the file is copied in the work directory.

```Dockerfile
FROM ubuntu:latest

RUN apt-get update && apt-get install -y \
    john

COPY . /app

WORKDIR /app

CMD ["bash"]
```

![enter image description here](https://i.imgur.com/rMQPqqc.png)

> Token : `abcdefg`

> Flag : `f2av5il02puano7naaf6adaaf`

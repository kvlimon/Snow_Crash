# Reverse a shift cipher

Here is the program provided by level09, to solve this level it is not necessary to understand how it works. However there is an interesting concept, the injection of code by the environment variable **$LD_PRELOAD** that I found 
in the main.

![enter image description here](https://i.imgur.com/h4SaCiD.png)

**$LD_PRELOAD** is an environment variable used on Linux to specify libraries to be loaded before all others when launching a program, thus allowing some functions to be substituted by custom implementations.

Here is a demonstration of what can be done :

```c
#include <stddef.h>
size_t strlen(const char *str)
{
    const char *s;
    for (s = str; *s; ++s);
    return ((s - str) + 1);
}
```

```bash
gcc -Wall -fPIC -shared -o libcustom_strlen.so custom_strlen.c
```

```bash
LD_PRELOAD=/chemin/vers/libcustom_strlen.so ./prog
```

After these operations, if you link your program to this shared library, you will have the "custom" **`strlen()`** and not the real one.

Anyway, to solve this level, you just have to understand how it manipulates the string passed as an argument and reverse the token.

![enter image description here](https://i.imgur.com/MKlNV4n.png)

The offset of a character is based on the distance from the beginning and the actual position.
Here is a program to reverse this encryption by offset.

```cpp
#include <iostream>
#include <fstream>

int
main()
{
    std::string filename = "token";
    std::ifstream file(filename);
    if (!file.is_open())
    {
        std::cerr << "Open failed: " << filename << std::endl;
        return 1;
    }
    char c;
    for (int i = 0; file.get(c); ++i)
    {
        std::cout << static_cast<char>(c - i);
    }
    file.close();
    return 0;
}
```

> Token : `f3iji1ju5yuevaus41q1afiuq`

> Flag : `s5cAJpM8ev6XHw998pRWG728z`

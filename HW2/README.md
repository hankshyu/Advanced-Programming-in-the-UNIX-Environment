# Monitor File Activities of Dynamically Linked Programs

- In this homework, we aim to practice **library injection** and **API hijacking**. 
- Implement a simple logger program that can show file-access-related activities of an arbitrary binary running on a Linux operating system. 
- You have to implement your logger in two parts:
  1. One is a logger program that prepares the runtime environment to inject, load, and execute a monitored binary program. 
  2. The other is a shared object that can be injected into a program by the logger using **LD_PRELOAD**. 
- You have to dump the library calls as well as the passed parameters and the returned values. 
- Detailed specification could be seen [here](https://github.com/hankshyu/Advanced-Programming-in-the-UNIX-Environment/blob/main/HW2/unix_hw2.pdf)


## Requirements

### Program Arguments
- Your program should work with the following arguments:
```
usage: ./logger [-o file] [-p sopath] [--] cmd [cmd args ...]
    -p: set the path to logger.so, default = ./logger.so
    -o: print output to file, print to "stderr" if no file specified
    --: separate the arguments for logger and for the command
```



### Monitored file access activities

The list of monitored library calls is shown below. It covers several functions we have introduced:
```
chmod chown close creat fclose fopen fread fwrite open read remove rename tmpfile write
```
## Hints
When implementing your homework, you may inspect symbols used by an executable. We have mentioned that you cannot see any symbols if they were symbol stripped (using ``strip`` command). However, you may consider working with the ``readelf`` command. For example, we can check the symbols that are unknown to the binary:
```
$ nm /usr/bin/wget
nm: /usr/bin/wget: no symbols
$ readelf --syms /usr/bin/wget | grep open
    72: 0000000000000000     0 FUNC    GLOBAL DEFAULT  UND freopen64@GLIBC_2.2.5 (2)
    73: 0000000000000000     0 FUNC    GLOBAL DEFAULT  UND iconv_open@GLIBC_2.2.5 (2)
   103: 0000000000000000     0 FUNC    GLOBAL DEFAULT  UND gzdopen
   107: 0000000000000000     0 FUNC    GLOBAL DEFAULT  UND fdopen@GLIBC_2.2.5 (2)
   119: 0000000000000000     0 FUNC    GLOBAL DEFAULT  UND open64@GLIBC_2.2.5 (2)
   201: 0000000000000000     0 FUNC    GLOBAL DEFAULT  UND fopen64@GLIBC_2.2.5 (2)
```
Alternatively, you may consider using ``nm -D`` to read symbols. Basically, we have two different symbol tables. One is the regular symbol table, and the other is the dynamic symbol table. The one removed by strip is the regular symbol table. So you will need to work with ``nm -D`` or ``readelf --syms`` to read the dynamic symbol table.

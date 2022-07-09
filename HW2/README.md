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

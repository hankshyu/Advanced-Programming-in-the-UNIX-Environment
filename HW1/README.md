# Implement a 'lsof'-like program

- In this homework, you have to implement the 'lsof' tool by yourself. 
- 'lsof' is a tool to list open files, it can be used to list all the files opened by processes running in the system. 
- The output of your homework is required to follow the spec strictly. 
- Detailed specification could be seen [here]()
## Program Arguments

1. Your program should work without any arguments. 
2. In the meantime, your program has to handle the following arguments properly:

- **-c REGEX**: a regular expression (REGEX) filter for filtering command line. For example         -c sh would match bash, zsh, and share.

- **-t TYPE**: a TYPE filter. Valid TYPE includes  REG,  CHR,  DIR,  FIFO,  SOCK, and   unknown. TYPEs other than the listed should be considered invalid. For invalid types, your program has to print out an error message Invalid TYPE option. in a single line and terminate your program.

- **-f REGEX**: a regular expression (REGEX) filter for filtering filenames.


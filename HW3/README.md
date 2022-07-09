# Extend the Mini Lib C to Handle Signals

- You have to extend the mini C library introduced in the class to support signal relevant system calls. 
- You have to implement the following C library functions in **x86 Assembly** and **C** using the syntax supported by yasm x86_64 assembler.

  1. setjmp: prepare for long jump by saving the current CPU state. In addition, preserve the signal mask of the current process.
  2. longjmp: perform the long jump by restoring a saved CPU state. In addition, restore the preserved signal mask.
  3. signal and sigaction: setup the handler of a signal.
  4. sigprocmask: can be used to block/unblock signals, and get/set the current signal mask.
  5. sigpending: check if there is any pending signal.
  6. alarm: setup a timer for the current process.
  7. write: write to a file descriptor.
  8. pause: wait for signal
  9. sleep: sleep for a specified number of seconds
  10. exit: cause normal process termination
  11. strlen: calculate the length of the string, excluding the terminating null byte ('\0').
  12. functions to handle sigset_t data type: sigemptyset, sigfillset, sigaddset, sigdelset, and sigismember

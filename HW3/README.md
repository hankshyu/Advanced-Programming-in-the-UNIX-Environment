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

## Hints

x86_64 system call table: It should be easy for you to find one on Internet. Here is the one we demonstrated in the course (x86_64 syscall table).
If you need precise prototypes for system calls, you may refer to an online Linux cross reference (LXR) site. For example, this page shows the official prototypes form the linux kernel (include/linux/syscalls.h).
You will have to define all the required data structures and constants by yourself. If you do not know how to define a data structure, try to find them from the Linux kernel source codes.
With LXR, you may also check how a system call is implemented, especially when an error code is returned from a system call. For example, here is the implementation for sys_rt_sigaction system call in the kernel. By reading the codes, you would know that passing an incorrect sigset_t size would lead to a negative EINVAL error code.
For implementing setjmp with a preserved process signal mask, the recommended data structure for x86_64 is given below:
typedef struct jmp_buf_s {
	long long reg[8];
	sigset_t mask;
} jmp_buf[1];
The minimal eight 64-bit values you have to preserve in the reg array are: RBX, RSP, RBP, R12, R13, R14, R15, and the return address (to the caller of setjmp). The current process signal mask can be preserved in the mask field.
To ensure that a signal handler can be properly called without crashing a process, you have to do the following additional setup in your implemented sigaction function as follows (illustrated in C language):
long sigaction(int how, struct sigaction *nact, struct sigaction *oact) {
	...
	nact->sa_flags |= SA_RESTORER;
	nact->sa_restorer = /* your customized restore routine, e.g., __myrt */;
	ret = sys_rt_sigaction(how, nact, oact, sizeof(sigset_t));
	...
}
The implementation of the __myrt function is simply making a system call to sigreturn (rax = 15).
Please notice that the sigaction data structure used in the C library may be different from that used in the kernel. If the user space and the kernel space data structure are inconsistent, you will have to perform the convertion in your wrapper functions.

test_code
=========

some codes for test

当使用setgid时，sigwaitinfo会出现段错误，而sigwait就没有这个问题；
推测与Linux NPTL使用前两个实时信号有关系；
strace结果如下：
mmap(NULL, 8392704, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_ANONYMOUS|MAP_STACK, -1, 0) = 0x7f4115cce000
brk(0)                                  = 0x1b43000
brk(0x1b64000)                          = 0x1b64000
mprotect(0x7f4115cce000, 4096, PROT_NONE) = 0
clone(child_stack=0x7f41164cdff0, flags=CLONE_VM|CLONE_FS|CLONE_FILES|CLONE_SIGHAND|CLONE_THREAD|CLONE_SYSVSEM|CLONE_SETTLS|CLONE_PARENT_SETTID|CLONE_CHILD_CLEARTID, parent_tidptr=0x7f41164ce9d0, tls=0x7f41164ce700, child_tidptr=0x7f41164ce9d0) = 14572
Receive signal. 10
rt_sigprocmask(SIG_BLOCK, [CHLD], Receive signal. 12
Receive signal. 34
Receive signal. 34
Receive signal. 36
Receive signal. 36
Receive signal. 64
[USR1 USR2 RT_2 RT_4], 8) = 0
Receive signal. 64
rt_sigaction(SIGCHLD, NULL, {SIG_DFL, [], 0}, 8) = 0
rt_sigprocmask(SIG_SETMASK, [USR1 USR2 RT_2 RT_4], NULL, 8) = 0
nanosleep({1, 0}, 0x7fffd3d74970)       = 0
tgkill(14571, 14572, SIGRT_1 <unfinished ...>
+++ killed by SIGSEGV +++
Segmentation fault

竟然使用了tgkill向线程发送了实时信号1；
具体原因不清楚；有待研究；

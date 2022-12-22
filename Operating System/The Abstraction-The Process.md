
<span style="color:yellow">Process:</span>
The definition:  a running program

<span style="color:yellow">Illusion:</span>
- by virtualizing the CPU
- running one process, then stopping it and running another, and so forth
- This basic technique, known as time sharing of the CPU, allows users to run as many concurrent processes as they would like
- the potential cost is performance, as each will run more slowly if the CPU(s) must be shared

<span style="color:yellow">Process API:</span>
- Create: must include some method to create new processes
- Destroy: As there is an interface for process creation, systems also provide an interface to destroy processes forcefully
- Wait: wait for a process to stop running
- Miscellaneous Control: other controls that are possible
- Status: get some status information
![[Pasted image 20221222154506.png]]

<span style="color:yellow">Process states:</span>
- Running: In the running state, a process is running on a processor. This means it is executing instructions
- Ready: In the ready state, a process is ready to run but for some reason the OS has chosen not to run it at this given moment.
- Blocked: In the blocked state, a process has performed some kind of operation that makes it not ready to run until some other event takes place.
![[Pasted image 20221222154419.png]]


<span style="color:yellow">Data Structures:</span>
To track the state of each process, for example, the OS likely will keep some kind of process list for all processes that are ready, as well as some additional information to track which process is currently running.

-----

fork(): The fork() system call is used to create a new process.

The process calls the fork() system call, which the OS provides as a way to create a new process. The odd part: the process that is created is an (almost) exact copy of the calling process. That means that to the OS, it now looks like there are two copies of the program running, and both are about to return from the fork() system call. The newly-created process (called the child, in contrast to the creating parent) doesn’t start running at main(), rather, it just comes into life as if it had called fork() itself.


wait(): parent to wait for a child process to finish what it has been doing






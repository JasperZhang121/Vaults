
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




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

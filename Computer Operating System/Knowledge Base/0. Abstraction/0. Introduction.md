
Operating Systems:Three Easy Pieces
- virtualization
- concurrency
- persistence

Language using:
- C

----
Running programe: (basics of the Von Neumann model of computing)
- the processor fetches an instruction from memory
- decodes it
- executes it
- moves on to the next instruction

<br />

<span style="color:yellow">Operating system</span> is a body of *software*:
responsible for making it easy to run programs, allowing programs to share memory, enabling programs to interact with devices, and other fun stuff like that.

<br />

<span style="color:yellow">Virtualization:</span>

Physical resource  -> virtual form -> <span style="color:red">A Virtual Machine </span>
Allows many programs to run -> <span style="color:red">A Resource Manager</span>

Running Many Programs At Once -> illusion:
- virtualizing the CPU
- policy of the OS

Virtualizing Memory:
- physical memory: an array of bytes
- read: must specify an address to be able to access the data stored there
- write: one must also specify the data to be written to the given address
- <span style="color:red">Memory is accessed all the time when a program is running</span>
- each instruction of the program is in memory too; thus memory is accessed on each instruction fetch
- each process accesses its own private virtual address space (sometimes just called its address space)
- A memory reference within one running program does not affect the address space of other processes (or the OS itself)

<br />

<span style="color:yellow">Concurrency:</span>
- multi-threaded programs
- instructions do not execute atomically (all at once) ->problem of concurrency

<br />

<span style="color:yellow">Persistence:</span>
- In system memory, data can be easily lost
- we need hardware and software to be able to store data persistently
- hardware comes in the form of some kind of input/output or I/O device
- software in the operating system that usually manages the disk is called the <span style="color:red">file system</span>
- OS does not create a private, virtualized disk for each application. Rather, it is assumed that often times, users will want to share information that is in files.
- to handle the problems of system crashes during writes, most file systems incorporate some kind of intricate write protocol, such as journaling or copy-on-write











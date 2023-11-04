# Procedure followed to run lottery scheduler in xv6
## Setup

Initially, to get the docker container up and running, install docker desktop on your PC.

Then, execute the below command in command prompt or terminal to download the required docker image.
```bash
$ docker pull grantbot/xv6
```

To run the docker image, run the below command in terminal or command prompt
```bash
$ docker run -it grantbot/xv6
```

Copy the provided xv6 folder to the directory /home/a/

Run the command to give execute permission to the files present in our xv6 folder
```bash
$ chmod -R +x xv6
```

Inside the shell in the ubuntu image, run the below command to go into our xv6 working directory
```bash
$ cd /home/a/xv6
```

To run the xv6 OS in qemu emulator with round-robin scheduler, run the below command
```bash
$ make
```
The above command is for building the OS

```bash
$ make qemu-nox
```
The above command is for running the xv6 OS

To run the process status system call, run the below command
```bash
$ ps    
Priority scheduler in use, change in ticket won't make a difference 
name     pid     state   priority        tickets
init     1       SLEEPING        2       10 
 sh      2       SLEEPING        2       10 
 ps      3       RUNNING         2       10
```

To create parent and child processes, run the below command
```bash
$ simpleprog&;           
$ Parent with pid 5 creating child with pid6
Child with pid 6 created
```

To show that parent and child processes are created,
```bash
$ ps
Priority scheduler in use, change in ticket won't make a difference 
name             pid     state   priority        tickets
init             1       SLEEPING        2       10 
 sh              2       SLEEPING        2       10 
 simpleprog              6       RUNNING         10      10 
 ps              9       RUNNING         2       10 
 simpleprog              5       SLEEPING        2       10 
```


# Nice command

To allocate priorities to the processes use the nice command with the syntax specifiede below,
```bash
$ nice
Usage: nice <process_id> <desired_priority>
```

Before executing nice command the process statuses and their priorities are as given below,
```bash
$ ps
Priority scheduler in use, change in ticket won't make a difference 
name     pid     state   priority        tickets
init     1       SLEEPING        2       10 
 sh      2       SLEEPING        2       10 
 simpleprog      7       RUNNING         10      10 
 ps      9       RUNNING         2       10 
 simpleprog      6       SLEEPING        2       10 
```

Executing nice command to change the priority to 10 of process with pid=6
```bash
$ nice 6 10
process_id=6, pr=10
```

After executing nice command the process statuses and their priorities are as given below,
```bash
$ ps
Priority scheduler in use, change in ticket won't make a difference 
name     pid     state   priority        tickets
init     1       SLEEPING        2       10 
 sh      2       SLEEPING        2       10 
 simpleprog      7       RUNNING         10      10 
 ps      12      RUNNING         2       10 
 simpleprog      6       SLEEPING        10      10 
```

# Pseudo random number generator
To test the prng and receive 1000 random numbers run the below command
```bash
$ testprng
```

# Lottery scheduler
## WHEN SWITCHING SCHEDULING POLICY ALWAYS RUN THE BELOW COMMAND
If you are already in the xv6 OS, first exit the xv6 OS using Ctrl+A and immediately press X

Then run,
```bash
$ make clean
```

To run the xv6 OS in qemu emulator with lottery scheduler, run the below command
```bash
$ make SCHEDULING_POLICY=LOTTERY
```
The above command is for building the OS

```bash
$ make qemu-nox
```
The above command is for running the xv6 OS

To verify the running of lottery scheduler,
```bash
$ ps    
Priority scheduler in use, change in ticket won't make a difference 
name     pid     state   priority        tickets
init     1       SLEEPING        2       10 
 sh      2       SLEEPING        2       10 
 ps      3       RUNNING         2       10
```

## To test lottery scheduler, run the below test cases

### ltest
```bash
$ ltest
starting test at 1 hours 49 minutes 15 seconds
spin with spin with 20 tickets ended at 1 hours 49 minutes 15 seconds
80 tickets ended at 1 hours 49 minutes 15 seconds
```

### ltest1
```bash
$ ltest1
All child processes with equal number of tickets have completed their tasks.
```

### ltest2
```bash
$ ltest2
Child 2 (Tickets: 30) has completed its custom tasks
Child 0 (Tickets: 10) has completed its custom tasks
Child 1 (Tickets: 20) has completed its custom tasks
All children with varying ticket values have finished their tasks
```


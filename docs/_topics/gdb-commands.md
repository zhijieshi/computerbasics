---
title: GDB
layout: default
description: Commonly used GDB commands
---

Read gdb manual for details.

We can always see the help message in gdb. For example, 

```
    help x
    help b
```

# Configure gdb

Add the following line into ~/.gdbinit to see instructions in Intel syntax.

```
    set disassembly-flavor intel
```

If necessary, we can add more gdb commands here.

We can also put commands in a file, say, saved-commands, and then use `source saved-commands` to execute all of them in gdb.

# Start gdb

Start gdb on a program main. Change main to whatever program
you want to inspect.

```
    $gdb main
```

# run program 
Run with arguments. Note this is inside gdb, not on the command line.

```
    run arg0 arg1 arg2 ...
```

or use `set args` command.

```
    set args arg0 arg1 arg2 ...

```
We can also use `start`, which will stop *at most convenient entry* (e.g., main()).

# Breakpoints

Set a breakpoint at a function main. Function names and line numbers
can be modified by filename, for example, `a.c:10`.

We can also add a condition to the break point.

```
    break main		# specify a function name
    break [n]           # specify a line number
    break [n] if [expr] # break at line 10 if i == 100
    break *0x08048449	# specify an address
```

Breakpoints can be saved and loaded in later sessions.

```
    save breakpoints filename
    source filename
```

# Control how program runs and when to stop again

```
    continue [n]
    step [n]
    next [n]
    stepi [n]
    nexti [arg]
    until [location]
    finish
```

# What you can do when program stops

Show the frame information on the stack.

Find out where we are.

```
    backtrace		# or bt  call stack
    frame n             # select a frame to inspect  
    info frame          # current frame (use frame command to switch)
    info locals         # local variabls
    info args           # arguments to the function
```

Check values with print  command.

```
    print /x &var		# var's address
    print /x var		# var's value
    print 100 + 200		# expressions
    print /x 100 + 200		# show results in hex
```

Examine the memory. Specify an address or varirable.

Can display in different formats.

```
    d - int
    c - char 
    s - string
    i -  instructions
```

Can include a modifier for size:

```
    b - byte
    h - halfword (16-bit value)
    w - word (32-bit value)
    g - giant word (64-bit value)
```

Examples: 

```
    # show 64 words in hex starting from where q is
    # located.
    x/64x &q		
    x/20xg  $rsp        # check the display 20 64-bits on the stack

    x/64xw 0x20000	# 64 words (32-bit) in hex format

    x/64xb 0x20000	# 64 bytes in hex format

    x/s string	    # examine the memory location associated with var s1 as a string
    x/10c string    # examine the first 10 chars in string
    x/8d string     # the ascii values of the first 8 chars of string

    x/d i	    # examine the memory location assoc with var i as an int 

    x/5i $rip	    # can also display instructions
```
    
Set the value of memory location

```
    set {int}0x8FF040 = 4
    set {int}&v=4
```

Check the instructions with disassemble command, or use x command. 

```
    disas starting_address ending_address
    disas 0x2000 
    disas function_name
    disas /m main
```

# Reverse

Sometimes we would like to undo the instructions, but we need to let gdb start the recording first.

```
record
# then we can reverse execution of instructions recorded
reverse-next[i]
reverse-step[i]
reverse-continue
reverse-finish
```



# Core dump

When there is a core dump, we can load the core into gdb and then study.

```
gdb program core
```
Sometimes, core dump is disabled by the admin. We can enable it, but instructions are
different for each kind of OS.

For Ubuntu 20.04, we can try:

```
# set the limit of core dump file (by default, soft limit is changed)
ulimit -c               # check the current soft limit on core size
ulimit -c unlimited     # set the max soft limit on core size

# if needed, change the hard limit
sudo ulimit -c unlimited -H

# check the current 
cat /proc/sys/kernel/core_pattern

# if needed, set the core dump file location, for example, to the current directory
sudo sysctl -w kernel.core_pattern=core.%u.%p.%t 
```

# Links

[The 15-minute video](https://www.youtube.com/watch?v=PorfLSr3DDI) 
shows that you can do a lot with gdb. 

There are many gdb cheat sheets on the Internet, for example, 
[link](https://cs-uob.github.io/COMS20012/materials/lecture1/GDBCheatSheet.pdf)


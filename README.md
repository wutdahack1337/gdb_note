# gdb_note
GDB Note vì có quá nhiều thứ để nhớ

# GDB is a very powerful dynamic analysis tool which you can use in order to understand the state of a program throughout its execution
Cái nào t xài nhiều thì t mới thêm vô

## 1. start, starti, run, attach \<PID\>, core \<PATH\>, continue
- start: to start a program, with a breakpoint set on "main"
- starti: to start a program, with a breakpoint set in "_start"
- run: to start a program, with no breakpoint set
- continue ('c' for short): in order to continue program execution until the program hits a breakpoint

## 2. info registers, print
- info registers: print the values for all your registers
- print/\<u\> ('p' for short): print a particular register's value. \<u\> is the unit size to display: 'd' (decimal), 'x' (hexadecimal), 's' (string), 'i' (instruction)

## 3. x, disassemble main, set disassembly-flavor intel
- x/\<n\>\<u\>\<f\> \<address\>: Examine the contents of memory. In this format \<u\> is the unit size to display, \<f\> is the format to display it in, and \<n\> is the number of elements to display
- disassemble main ("disas main" for short): print all of the instructions of main
- set disassembly-flavor intel: CORRECT assembly syntax

## 4. stepi, nexti, finish, break *\<address\>, display/\<n\>\<u\>\<f\>, layout regs
- stepi \<n\> ("si" for short): in order to step forward one instruction
- nexti \<n\> ("ni" for short): in order to step forward one instruction, while stepping over any function calls
- finish: in order to finish the currently executing function
- break *\<address\>: to set a breakpoint at the specified-address
```
break *main
break *main + 1337
```
- display/\<n\>\<u\>\<f\>: always show you what you want to see
- layout regs: put gdb into its TUI mode and show you the contents of all of the registers, as well as nearby instruction

## 5. ~/.gdbinit, gdb srcipting
- ~/.gdbinit: some commands be always executed for any gdb session by putting them in this. "set disassembly-flavor intel" for sure.
- gdb srcipting: example "debug.gdb", we can using the flag "-x \<PATH_TO_SCRIPT\>". Within gdb scripting, a very powerful construct is breakpoint commands. Consider the following gdb script:
```
  start
  break *(main+42)
  commands
    x/gx $rbp-0x32
    continue
  end
  continue
```
```
  start
  break *(main+42)
  commands
    silent
    set $local_variable = *(unsigned long long*)($rbp-0x32)
    printf "Current value: %llx\n", $local_variable
    continue
  end
  continue
```
## 6. set
- set: Modify the state of your target program.
```
start

break *(main+686)
commands
        set $rdx = $rax
        continue
end

continue
```
```
set $rdi = 0 // to zero out $rdi
set *((uint64_t *) $rsp) = 0x1234 // to set the first value (64 bits) on the stack to 0x1234
set *((uint16_t *) 0x31337000) = 0x1337 // to set 2 bytes (16 bits) at 0x31337000 to 0x1337
```
```
Suppose your target is some networked application which reads from some socket on fd 42. Maybe it would be easier for
the purposes of your analysis if the target instead read from stdin. You could achieve something like that with the
following gdb script:

  start
  catch syscall read
  commands
    silent
    if ($rdi == 42)
      set $rdi = 0
    end
    continue
  end
  continue
```
## 7. call (void)function()
- call (void)function(): running within this elevated instance of gdb gives you elevated control over the entire system

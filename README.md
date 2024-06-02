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
- print/\<u\> ('p/\<u\>' for short): print a particular register's value. \<u\> is the unit size to display: 'd' (decimal), 'x' (hexadecimal), 's' (string), 'i' (instruction)

## 3. x, disassemble main, set disassembly-flavor intel
- x/\<n\>\<u\>\<f\> \<address\>: Examine the contents of memory. In this format \<u\> is the unit size to display, \<f\> is the format to display it in, and \<n\> is the number of elements to display
- disassemble main ("disas main" for short): print all of the instructions of main
- set disassembly-flavor intel: CORRECT assembly syntax

## 4. si, ni, finish, break

## 5. ~/.gdbinit, gdb srcipting
- ~/.gdbinit: some commands be always executed for any gdb session by putting them in this. "set disassembly-flavor intel" for sure.
- gdb srcipting: example "debug.gdb", we can using the flag "-x \<PATH_TO_SCRIPT\>". Within gdb scripting, a very powerful construct is breakpoint commands. Consider the following gdb script:
```
  start
  break *main+42
  commands
    x/gx $rbp-0x32
    continue
  end
  continue
```
```
  start
  break *main+42
  commands
    silent
    set $local_variable = *(unsigned long long*)($rbp-0x32)
    printf "Current value: %llx\n", $local_variable
    continue
  end
  continue
```

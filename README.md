# Party Bypass by Team Nullworks Reversal by Mrsteyk

## Tools being used: gdb, PEDA, knowledge

## Instructions:

We all know that you need to mprotect .text regions to make 'em writable.
We will use this piece of knowledge to our advantage!

* exec dat shit:
```
set $dlopen = (void*(*)(char*, int)) dlopen
b mprotect
call $dlopen("/home/mrsteyk/pb.so", 1)
```
* Then press `c` a couple times untill you will see some warnings
* You are in mprotect func, which for some reason aint being "destroyed" from import
* Type `finish` and look for next call - its memcpy function, which was "destroyed"
* Put a break on it
* Type `c`
* Look @ stack for args and substract the .text base from dest and get bytes from src array with pushed size (be sure to not fuck up like me)
* Repeat prev 2 steps
...
BOOM! DONE!

Table:

| Offset from .text | Name      | Byte Array & ReMarks                                                                 |
| ----------------- | :-------: | ------------------------------------------------------------------------------------ |
| 0xb4355d          | patch  1  | 0x67 0x76 // fucked up; scrolling history is only 512 lines // right one - 0x90 0x90 |
| 0xb4303f          | patch  2  | 0xbb 0x01 0x00 0x00 0x00 0x90                                                        |
| 0xb4077a          | patch  3  | 0x67 0x76 // right one - 0x90 0xe9                                                   |
| 0xb45181          | patch  4  | 0xc6 0x85 0x8f 0xfe 0xff 0xff 0x01                                                   |
| 0xb481c3          | patch  5  | 0x8b 0x85 0xf4 0xfd 0xff 0xff                                                        |
| 0xb48244          | patch  6  | 0x90 0x90 0x90 0x90 0x90 0x90                                                        |
| 0xb48251          | patch ?7? | 0x67 0x76 // right one - 0x0f 0x8e                                                   |

---

# Follow up as of 4th of jan

```
--- ---------------------------- ---
--- new version as of 4th of jan ---
--- ---------------------------- ---
```

* Exec:
```
set $dlopen = (void*(*)(char*, int)) dlopen
b mprotect
call $dlopen("/home/mrsteyk/pb.so", 1)
```
* SAME SHIT EXCEPT YOU PRESS `c` ABOUT 6 TIMES AND IGNORE FIRST TOTALLY RIGHT PATCH, PATCHING `c7 44` to `c7 44`! (genius, amirite?)

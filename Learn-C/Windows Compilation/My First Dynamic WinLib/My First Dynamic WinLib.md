# My First Dynamic WinLib

1) create a program that count the number of char in a string put this program inside MyStrLib.c and create his header
2) change this code in dynamic library (libstr.dll) (read the [link](https://stackoverflow.com/questions/1130479/how-to-build-a-dll-from-the-command-line-in-windows-using-msvc))
3) create a main.c in the same directory of your .dll and
paste the following code :
```c
#include "my_first_static_lib.h"
#include<stdio.h>

int main()
{
    printf("the number is %d", countstr("test this sentance"));
}
```
4) run the following command ```cl main.c my_first_static_lib.dll```
7) add your lib in a /home/Mylib directory and put this directory inside LD_LIBRARY_PATH
8) recompile the main with your new path 
9) launch the binary 
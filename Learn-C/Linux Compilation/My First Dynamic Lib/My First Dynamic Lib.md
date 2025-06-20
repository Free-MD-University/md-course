# My First Dynamic Lib

1) create a program that count the number of char in a string put this program inside MyStrLib.c and create his header
2) change this code in dynamic library (libstr.so) (read the pdf)
3) create a main.c in the same directory of your .so and
paste the following code :
```c
#include <stdio.h>
#include "MyStrLib.h"

int main()
{
    char* test = "test number of char";
    printf("%d",strcount(test));
}
```
4) run the following command ```gcc main.c -L. -lstr```
5) try to compile main with ```gcc main.c -L. -lstr```
6) You have a problem ? why ? (find in the pdf)
7) add your lib in a /home/Mylib directory and put this directory inside LD_LIBRARY_PATH
8) recompile the main with your new path 
9) launch the binary 
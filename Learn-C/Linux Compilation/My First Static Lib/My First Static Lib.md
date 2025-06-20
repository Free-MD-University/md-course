# MFSL:
    1) I give a static lib for linux os lib_print.a and his .h file, create a main that call the function of the print_me.h
    2) create the executable by linking the static lib 
    3) understand gcc static library order (for exemple `gcc main.o -L. -l_print` will work but not `gcc -L. lib_print.a main.o`)


## Course:
[Cours](https://dev.to/iamkhalil42/all-you-need-to-know-about-c-static-libraries-1o0b) OR 
[HERE](https://www.geeksforgeeks.org/static-vs-dynamic-libraries/)

### Static Library Order: 
read the pdf given in the course.


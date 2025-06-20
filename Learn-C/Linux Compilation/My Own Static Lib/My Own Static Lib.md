# My Own Static Lib

1) create a C file with his header (.h) that contains the function:
    - print_me : that print your name
2) create a C file with his header (.h) that contains the function:
    - dont_use_me : that use the exit function with 1 as parameter
3) use gcc compileur (or other of your choice) with the -c option to compile them in an object file (do it with 2 separate command, and in one command)

4) create a static library (man ar (with -rc option))

5) create a main.c that use the print_me function and exit from the code with dont_use_me function.

6) use ar -t to list every object in the library
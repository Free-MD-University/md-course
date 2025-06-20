# My First Static WinLib
    1) to use the tools of c compiler in windows we need to install vsstudio (for me it will vs community 2022)
    2) we need to enter in the Visual Studio 2022 Developer Command Prompt to do so launch the bat inside C:\Program Files\Microsoft Visual Studio\2022\Community\VC\Auxiliary\Build (the path van change between vsstudio version ) from a cmd and not from powershell it will not work 
    3) when the bat is launched there will be new command available like cl read his help 
    4) create a main.c that use the function print_me that is inside the static lib given use /I option (we have to use the full path to the library in two way first /I folder_to_lib full_path_of_lib)
     
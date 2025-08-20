# Volability
Volatility is the world's most widely used framework for extracting digital artifacts from volatile memory (RAM) samples.

the frameworks is write in python so it's real easy for multiplatform .
to extract memory there are multiple choise depending the host :

# FOR VM

there is a file with a memory f the ram 
VMWare - .vmem
Hyper-V - .bin
Parallels - .mem
VirtualBox - .sav file 

# for bare metal host 

FTK Imager
DumpIt.exe
win32dd.exe / win64dd.exe
Memoryze
FastDump


# command 
python3 vol.py -f <file> windows.info / linux.info / mac.info
python3 vol.py -f <file> windows.pslist # use list of process according to the OS 
python3 vol.py -f <file> windows.psscan # list by scan the ram for process
python3 vol.py -f <file> windows.pstree # list pstree
python3 vol.py -f <file> windows.dlllist
python3 vol.py -f <file> windows.malfind
python3 vol.py -f <file> windows.yarascan
python3 vol.py -f <file> windows.handles

# special registry
a special regitry with 
SAM/SECURITY/DEFAULT/Sysmte/Software : %SystemRoot%\system32\config
SAM: Stores the security account manager information, including user account data.
SECURITY: Contains system-level security settings.
SOFTWARE: Stores software-related configuration settings.
SYSTEM: Holds information about the system's hardware and configuration.
DEFAULT: Contains the registry settings for the default user profile.
NTUSER.DAT (mounted on HKEY_CURRENT_USER when a user logs in) --> store last files
USRCLASS.DAT (mounted on HKEY_CURRENT_USER\Software\CLASSES) --> sotre user preference 
# Amcache
Apart from these files, there is another very important hive called the AmCache hive. This hive is located in C:\Windows\AppCompat\Programs\Amcache.hve. Windows creates this hive to save information on programs that were recently run on the system.

Background Activity Monitor or BAM keeps a tab on the activity of background applications. Similar Desktop Activity Moderator or DAM is a part of Microsoft Windows that optimizes the power consumption of the device these can be foun in SYSTEM\CurrentControlSet\Services\bam\UserSettings\{SID}


# File History
NTUSER.DAT\Software\Microsoft\Windows\CurrentVersion\Explorer\RecentDocs



#KAPE
KAPE is a live data acquisition and analysis tool which can be used to acquire registry data.

# AUtopsy
Autopsy  gives you the option to acquire data from both live systems or from a disk image. 

# Fat32
old filesystem used by windows . file are limited to 4GB . 
remplaced by exffat

# Exfat
default FS of SD card . support PB files and 2 Millions of file in a folder.

# NTFS

NTFS introduced by microsft . have a jounal of metadata change, support ACL, DATA Stream..
$MFT
The $MFT is the first record in the volume. The Volume Boot Record (VBR) points to the cluster where it is located. $MFT stores information about the clusters where all other objects present on the volume are located. This file contains a directory of all the files present on the volume.

$LOGFILE
The $LOGFILE stores the transactional logging of the file system. It helps maintain the integrity of the file system in the event of a crash.

$UsnJrnl
It stands for the Update Sequence Number (USN) Journal. It is present in the $Extend record. It contains information about all the files that were changed in the file system and the reason for the change. It is also called the change journal.


To check the HD we can use MFT Explorer is one tools of Eric Zimmerman. to run it we use the command MFTECmd.exe.
we can create a result in csv format with MFTECmd.exe -f <path-to-$MFT-file> --csv <path-to-save-results-in-csv> we can use EZviewr to view result

# Recovering file 
we can use autopsy to recover files . we can see removeed and hodden file from a system

# execution 

## Windows Prefetch 

when a program is used a lot of time, the program will store a cache copy named prefetch located in C:\Windows\Prefetch.
we can use PECmd.exe to read this prefetch 
## Windows 10 Timeline:
Windows 10 stores recently used applications and files in an SQLite database called the Windows 10 Timeline
C:\Users\<username>\AppData\Local\ConnectedDevicesPlatform\{randomfolder}\ActivitiesCache.db
## Windows Jumplist
Windows introduced jump lists to help users go directly to their recently used files from the taskbar. We can view jumplists by right-clicking an application's icon in the taskbar, and it will show us the recently opened files in that application. This data is stored in the following directory:

C:\Users\<username>\AppData\Roaming\Microsoft\Windows\Recent\AutomaticDestinations

we can use JLECmd.exe to parse Jump Lists.

# Shortcut

Windows creates a shortcut file for each file opened either locally or remotely. The shortcut files contain information about the first and last opened times of the file and the path of the opened file, along with some other data. Shortcut files can be found in the following locations:

C:\Users\<username>\AppData\Roaming\Microsoft\Windows\Recent\

# IE/Edge history
An interesting thing about the IE/Edge browsing history is that it includes files opened in the system as well, whether those files were opened using the browser or not. Hence, a valuable source of information on opened files in a system is the IE/Edge history. We can access the history in the following location:

C:\Users\<username>\AppData\Local\Microsoft\Windows\WebCache\WebCacheV*.dat

# External Device

 This log is present at the following location:

C:\Windows\inf\setupapi.dev.log
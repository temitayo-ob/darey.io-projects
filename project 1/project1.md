# LINUX PRACTICE PROJECT BY TEMITAYO OBISAKIN 

This is an introduction to linux which is  an open source unix operating system based on the linux kernel, focusing on the linux commands used in navigation on the CLI (a console that interacts with the system via texts and processes).

Linux commands are programs executed on a terminal similar to the windows command prompt application, they are executed by pressing enter at the end of the line and are used to perform various tasks like user management and file manipulation.

- the linux general syntax is ---- `CommandName [option(s)] [parameter(s)]`

the three common parts are the command name (Rule to be performed), option (changes the command's operation and invoked by a hyphen) and parameter (Specifies information for the command).

**Note--Linux commands are case sensitive.**

## file manipulation
### 1 sudo command (super user do)
This is a common linux command that lets you perform tasks that require administrative or root permissions. 
- the general syntax for a sudo command is ---- `sudo (command e.g apt upgrade)`.

An example of a sudo command and its result is attached below 

`sudo apt upgrade`

![sudo commamd](./image/sudo%20commamd.jpg)

Options can also be added such as : -k , -g and -h to help modify the command's operation. 

### 2 pwd command
This command is used to find the path of your present working directory.
- it uses the following syntax --- `pwd [option]`

Example of its application is attached below by simply typing `pwd`

![pwd commamd](./image/pwd%20commamd.jpg)

options available are -l and -P

### 3 cd command
The cd command is used to navigate through linux files and directories, using the cd command without an option directs you to the home folder and can mainly be used by users that have sudo privileges.

the command is gernerally used by typing cd followed by the directories path as shown below.

![cd command](./image/cd%20command.jpg)

shortcuts include 
- cd~[username]  --- goes to another users home directory
- cd .. --- moves up one directory
- cd-  --- returns you to your previous directory

### 4 ls command 
To list files and directories within a system the ls command is used, can be used to show the contents of a current directory being worked on and also other directires by stating the path of the directory in question.for example using the 
ls /home/ubuntu command showing the path.

![ls command](./image/ls%20command.jpg)

other options include -R(includes sub directories), -a(shows hidden files), -lh(shows file sizes)

### 5 cat command (concatenate)
This command combines and writes file content to the standard output. 
- general syntax for the cat command is --- `cat [file name]`

![cat command](./image/cat%20command.jpg)

other ways to use this command includes:

- `cat filename1.txt filename2.txt > filename3.txt` ---- (merges result of filename1 and filename2 and stores the output in filename3)

- `tac filename.txt` ---- (Displays result in reverse order).

![tac command](./image/tac%20command.jpg)

### 6 cp command
The cp command is used to copy files or directories and their content,an example of copying a file from one directory to another by typing cp followed by the file name and destination directory. see below

![cp command](./image/cp%20command.jpg)

other ways to use the cp command includes:

- `cp filename1.txt filename2.txt filename3.txt /home/username/Documents`  ---- (to copy multiple files to a directory) 

- `cp filename1.txt filename2.txt` --- (to copy contents of one file to another file in thesame directory)

- `cp -R /home/username/Documents/home/username/Documents_backup` --- (to copy an entire directory)

### 7 mv command 
Used primarily to move and rename files and directories. by typing mv followed by the filename and destination directory as shown pictured.

![mv command](./image/mv%20command.jpg)

as shown above the mv command can also be used to rename file with the general syntax being --- `mv [filename] [new filename]`

### 8 mkdir command
This command is used to create one or multiple directories at once and to set permissions for each of them.

- The general syntax is :
`mkdir [option] directory_name`

pictured below is the application of the command to create a directory named music using :
mkdir Music
and secondly to create a directory called songs within the music folder using:
mkdir Music/Songs

![mkdir command](./image/mkdir%20command.jpg)

options for the mkdir command includes:
- -p --- to create a directory between two existing folders
- -m --- sets file permissions
- -v --- prints a message for each directory created.

### 9 rmdir command
The rmdir command is used  to permernently delete an empty directory,the user running this command should have sudo priviedges in parent directory.

for example to remove the song subdirectory and its main folder we use:

rmdir -p Music/Songs
![rmdir command](./image/rmdir%20command.jpg)

### 10 rm command
The rm command is used to delete files within a directory 

- the general syntax for this command is : `rm filename`

- to remove multiple files : `rm filename1 filename2 filename3`

see examples of its application below:

![rm command](./image/rm%20command.jpg)
Other options include:
- -i --- for confirmation before deleting a file
- -f --- to remove without confirmation 
- -r --- deletes recursively'

### 11 touch command 
This command is used to create an empty file or generate and modify a timestamp in the linux command line.

- General syntax is : `touch filename.extention`

![touch command](./image/touch%20command.jpg)

### 12 locate command 
The locate command is used to find files within the database system, using the -i option turns off case sensitivity so u can search for a file without knowing its exact name.

![locate command](./image/locate%20command.jpg)

### 13 find command 
The find command is used to search for files within a specific directory and perform subsequent operations.

- the general syntax is: `find [option] [path] [expression]`

for example to find a file called testfile1 within the home directory and subfolders is: 
`find /home -name testfile1 `

![find command](./image/find%20command.jpg)
other variations include :
-name filename.txt --- to find files in current directory

### 14 grep command (global regular expression print)
This command lets you find a word by searching through all texts in a specific file, and prints all lines that contain the specific pattern. It helps to filter through large log files.
for example to search for the word this in testfile3 : `grep this testfile3`

![grep command](./image/grep%20command.jpg)
output would display lines that contin the word this. 

### 15 df command 

The df command is used to report the system's disk space usage in percentage or kilobytes. 
- general syntax : `df [options] [file]`

to see current directory's system disk space usage in a human readable format:
`df -h`

![df command](./image/df%20command.jpg)
other options include 
- -m --- displays information in MegaBytes's
- -k --- displays in kilobytes
- -t --- shows file system type in a new window

### 16 du command
du command is used to check how much space a file or directory takes up.you must specify the path when using the du command.

![du command](./image/du%20command.jpg)
other options include :
- -s --- total size of a specified folder
- -m --- diplays in megabytes
- -k --- displays in kilobytes
- -h --- informs on the last modification date 

### 17 head command 
This command views the first 10 lines of a text, adding an option allows for changing the number of lines displayed.

- general syntax is : `head [option] [file]`

example of its application is illustrated below

![head command](./image/head%20command.jpg)
options include :
- -n / -lines --- prints  first customized number of lines. 
- -c / -bytes --- prints first customized number of bytesof each file.
- -q / -quiet --- will not print headers specifying the file name. 

### 18 tail command 
This command displays the last 10 lines of a file.

- general syntax is :`tail [option] [file]`

example attached below

![tail command](./image/tail%20command.jpg)

### 19 diff command (difference)
The diff command compares the contents of two files line by line,It displays the parts that do not match. 

- The general syntax is: `diff [option] file1 file2`

illustration of its usage :

![diff command](./image/diff%20command.jpg)
options include 
- -c --- displays difference in context form.
- -u --- displays output without redundant.information. 
- -i --- removes case sensitivity.

### 20 tar command
tar command archives files into a tar file with optional compression. 

- the general syntax is: `tar [options][archive_file] [file or directory to be archived]`

![tar command](./image/tar%20command.jpg)
options include 
- -x --- extracts a file 
- -t --- lists content of a file
- -u --- archives and adds to an existing archived file 

## File permissions and ownership

### 21 chmod command 
This command modifies a file or directory's read, write and execute permissions.

- the syntax is : `chmod [option] [permission] [file_name]`

for example to allow read,write, and execute permissions for group members and others on a file :(numeric value is 777)

![chmod command](./image/chmod%20command.jpg)

other options include 
- c --- displays info when a change i made 
- -f --- suppresses errors

### 22 chown command
This command allows changes to the ownership of a file, directory or symbolic link to a specified username.
- basic syntax is: `chown [option] owner[:group] file(s)`

![chown command](./image/chown%20command.jpg)

### 23 jobs command 
jobs are processes started from the shell, this command shows all running processes and their statuses. 

- syntax :`jobs [options] jobID`

to check status of jobs simply type jobs. 
note -- i dont have any current jobs running so the command returns empty 
![jobs command](./image/jobs%20command.jpg)

options include : 
- -i --- lists process id's and information.
- -n --- jobs with changed statuses since last notification.
- -p -- process id's only.

### 24 kill command 
Used to terminate an unresponsive program manually. to kill a program the process id is used and can be gotten by typing this command in the terminal :`ps ux`.

- general syntax is : `kill [signal_option] pid`

**signals**
- sigterm --- stops programs and gives time to save its progress
- sigkill --- forces programs to stop 

### 25 ping command 
Ping command is used to check if a network or server is reachable, and troubleshoot connectivity issues. 

- General format is: `ping [option] [hostname_or_IP_address]`

below is a connection check for google.com
![ping command](./image/ping%20command.jpg)

### 26 wget command 
This command is used to download files from the internet, 

- its general syntax is : `wget [option] [url]`

an example to dowload wordpress is below : wget https://wordpress.org/latest.zip
![wget command](./image/wget%20command.jpg)

### 27 uname command 
Prints information about tyour linux system and hardware like machine name, operating system and kernel. 

- Its basic syntax is: `uname [option]`

options include -a(system info), -s(kernel name) and -n(system's node hostname).
![uname command](./image/uname%20command.jpg)
 
### 28 top command 
Displays all running processes and a dynamic real time view of the current system. Can be used to identify and terminate a process that may use too many system recources. simply enter top into the CLI

![top command](./image/top%20command.jpg)

### 29 history command 
This command displays up to 500 previously executed commands allowing reuse, requires sudo priviledges to achieve this. 

- syntax : ``history [option]`

options include 
- -c --- clears history
- -d --- deletes entry at offset position 
- -a --- appends history lines

![history command](./image/history%20command.jpg)

### 30 man command 
Provies a user manual of commands or utilities u can run on the terminal,

- General syntax is :`man [command_name]`

to specify displayed section : man [option] [section_number] [command_name]
![man command](./image/man%20command.jpg)

### 31 echo command 
Displays a line of text or string using the standard output.
- basic syntax: `echo [option] [string]`

![echo command](./image/echo%20command.jpg)

### 32 zip, unzip command 
Zip is used to compress files into a zip file, also useful for archiving files and directories. 

- general syntax: `zip [options] zipfile file1 file2….`

unzip extracts files from a zip file, 
- general syntax :`unzip [option] file_name.zip`

as shown in the illustration 
![zip command](./image/zip%20command.jpg)

### 33 hostname command
This is used to show the system's hostname,

- General syntax is : `hostname [option]`

can also be used with various flags
![hostname command](./image/hostname%20command.jpg)

- -i --- displays ip address 
- -a --- displays hostname alias
- -A --- displays FQDN

### 34 useradd, userdel commands
useradd is used to create a new user account, that can be passworded, it can only be executed by users with sudo priviledges.

general syntax for useradd and password to be inputed simultaneously

- useradd --- `useradd [option] username`
- password --- `passwd the_password_combination`

userdel deletes a user account 
- syntax is : `userdel username`

![useradd command](./image/useradd%20command.jpg)

### 35 apt-get command 
Tool for handling advanced package tool (apt) libraries in linux. allows retrieving of information and bundles from authenticated sources to manage, update, remove and install software and its dependencies.

- main syntax is : `apt-get [options] (command)`

![apt-get command](./image/apt-get%20command.jpg)

### 36 nano, vi, jed commands
nano, vi and jed are text editors that allows users to edit and manage files.

nano denotes keywords that can work with most languages : `nano [filename]`

![nano command](./image/nano%20command.jpg)

vi uses two operating modes which are insert and command.insert is used to edit and create text files, while command perfomes various operations such as saving, opening and copying a file: `vi [filename]`

![vi command](./image/vi%20command.jpg)

jed uses a dropdown menu for navigation to carryout operations without keyboard combinations or commands. simply opened using : `jed `

![jed command](./image/jed%20command.jpg)

### 37 alias, unalias commands 
alias command allows users to create shortcuts for commands, filename or text.
- alias syntax : `alias Name=String`

unalias command deletes an existing alias 
- unalias syntax : `unalias [alias_name]`

![alias command](./image/alias%20command.jpg)

### 38 su command (switch user)

su command allows user to run a command s a different user, changes the administrative account in the current log-in session.

- General syntax : `su [options] [username [argument]]`

![su command](./image/su%20command.jpg)

### 39 htop command 
This is an interactive program that monitors system resources and server processes in real time. similar to the top command but consist of many improvements and additional features such a mouse operation and visual indicators.
- htop syntax : `htop [options]`

![htop command](./image/htop%20command.jpg)

### 40 ps command (process status) 
Takes a snapshot of all running proceses in your system. Displays information like, the process id (PID), terminal type (TTY), running time (TIME).

![ps command](./image/ps%20command.jpg)


Thank you.


# LINUX PRACTICE PROJECT BY TEMITAYO OBISAKIN 

This is an introduction to linux which is  an open source unix operating systems based on the linux kernel, focusing on the linux commands used in navigation on the CLI (a console that interacts with the system via texts and processes).

Linux commands are programs executed on a terminal similar to the windows command prompt application, they are executed by pressing enter at the end of the line and are used to perform various tasks like user management and file manipulation.

the linux general syntax is ---- CommandName [option(s)] [parameter(s)]
the three common parts are the command name (Rule to be performed), option (changes the command's operation and invoked by a hyphen) and parameter (Specifies information for the command).

## file manipulation
### 1 sudo command (super user do)
This is a common linux command that lets you perform tasks that require administrative or root permissions. 
the general syntax for a sudo command is ---- sudo (command e.g apt upgrade).

An example of a sudo command and its result is attached below 

sudo apt upgrade

![sudo commamd](https://github.com/temitayo-ob/darey.io-projects/assets/145216353/48e72b9b-f18c-4db2-a659-019438530e3d)

Options can also be added such as : -k , -g and -h to help modify the command's operation. 

### 2 pwd command
this command is used to find the path of your present working directory.
it uses the following syntax --- pwd [option]

Example of its application is attached below by simply typing pwd

![pwd commamd](https://github.com/temitayo-ob/darey.io-projects/assets/145216353/7bdf5808-66c4-480e-84e4-35cc5281619c)

options available are -l and -P

### 3 cd command
the cd commnd is used to navigate through linux files and directories, using the cd command without and option directs you to the home folder and can mainly be used by users that have sudo privileges.

the gernerally used by typing cd followed by the directories path as shown bellow.

![cd command](https://github.com/temitayo-ob/darey.io-projects/assets/145216353/779e7cd3-9436-4e9a-b3d2-804631f102c7)

shortcuts include 
cd~[username]  --- goes to another users home directory
cd .. --- moves up one directory
cd-  --- returns you to your previous directory

### 4 ls command 
to list files and directories within a system the ls command is used.can be used to show the contents of a current directory being worked on and also other directires by stating the path of the directory in question.for examole using the 
ls /home/ubuntu command showing the path.

![ls command](https://github.com/temitayo-ob/darey.io-projects/assets/145216353/6b84df28-956b-4a42-bfb3-9bd36aea79f9)

other options include -R(includes sub directories), -a(shows hidden files), -lh(shows file sizes)

### 5 cat command (concatenate)
this command combines and writes file content to the standard output. general syntax for the cat command is --- cat [file name]

![cat command](https://github.com/temitayo-ob/darey.io-projects/assets/145216353/aad18cf5-2656-423e-bb94-1c700f8d5b89)

other ways to use this command includes:

cat filename1.txt filename2.txt > filename3.txt ---- (merges result of filename1 and filename2 and stores the output in filename3)

tac filename.txt ---- (Displays result in reverse order).

### 6 cp command
The cp command is used to copy files or directories and their content,an example of copying a file from one directory to another by typing cp followed by the file name and destination directory. see below

![cp command](https://github.com/temitayo-ob/darey.io-projects/assets/145216353/bf989128-438a-40e2-9227-0dce72c7d1f0)

other ways to use the cp command includes:

cp filename1.txt filename2.txt filename3.txt /home/username/Documents  ---- (to copy multiple files to a directory) 

cp filename1.txt filename2.txt --- (to copy contents of one file to another file in thesame directory)

cp -R /home/username/Documents /home/username/Documents_backup --- (to copy an entire directory)

### 7 mv command 
Used primarily to move and rename files and directories. by typing mv followed by the filename and destination directory as shown pictured.

![mv command](https://github.com/temitayo-ob/darey.io-projects/assets/145216353/65a26230-2316-403b-958a-4d628b6a0f69)

as shown above the mv command can also be used to rename file with the general syntax being --- mv [filename] [new filename]

### 8 mkdir command
This command is used to create one or multiple directories aqt once and to set permissions for each of them.
The general syntax is :
mkdir [option] directory_name
pictured below is the application of the command to create a directory names music using :
mkdir Music
and secondly to create a directory called songs within the music folder using:
mkdir Music/Songs

![mkdir command](https://github.com/temitayo-ob/darey.io-projects/assets/145216353/01ae1f33-b3ce-4370-bec9-d46056b2aef5)

options for the mkdir command includes:
-p --- to create a directory between two existing folders
-m --- sets file permissions
-v --- prints a message for each directory created.

### 9 rmdir command
The rmdir command is used  to permernently delete an empty directory,the user running this command should have sudo priviedges in parent directory.

for example to remove the song subdirectory and its main folder we use:

rmdir -p Music/Songs
![rmdir command](https://github.com/temitayo-ob/darey.io-projects/assets/145216353/67c077a9-e462-47f8-899c-942d63660f3a)

### 10 rm command
The rm command is used to delete files within a directory 

the general syntax for this command is : rm filename
to remove multiple files : rm filename1 filename2 filename3

see examples of its application below:
![rm command](https://github.com/temitayo-ob/darey.io-projects/assets/145216353/b014d1c7-ef07-426f-8cf4-1e1a34d5adcf)
Other options include:
-i --- for confirmation before deleting a file
-f --- to remove without confirmation 
-r --- deletes recursively'

### 11 touch command 
This command is used to create an empty file or generate and modify a timestamp in the linux command line.

General syntax is : touch filename.extention
![touch command](https://github.com/temitayo-ob/darey.io-projects/assets/145216353/58ff5bbd-0cbe-4ff1-af8e-84bbb9057c8d)

### 12 locate command 
the locate command is used to find files within the database system.using the -i option turns off case sensitivity so u can search for a file without knowing its exact name.
![locate command](https://github.com/temitayo-ob/darey.io-projects/assets/145216353/68a12361-1cc2-4bef-893f-82c86ab1c46e)

### 13 find command 
The find command is used to search for files within a specific directory and perform subsequent operations.
the general syntax is:       find [option] [path] [expression]
for example to find a file called testfile1 within the home directory and subfolders is: 
find /home -name testfile1
![find command](https://github.com/temitayo-ob/darey.io-projects/assets/145216353/9957f68b-8159-409a-8ebb-91c6a645693b)
other variations include :
-name filename.txt --- to find files in current directory

### 14 grep command (global regular expression print)
this command lets you find a word by searching through all texts in a specific file, and prints all lines that contain the specific pattern. It helps to filter through large log files.
for example to search for the word this in testfile3 :grep this testfile3
![grep command](https://github.com/temitayo-ob/darey.io-projects/assets/145216353/f2e51990-f8ee-403f-a29b-4aec3945eaf7)
output would display lines that contin the word this. 

### 15 df command 

to report the system's disk space usage in percentage or kilobytes. 
general syntax : df [options] [file]

to see current directory's system disk space usage in a human readable format:
df -h
![df command](https://github.com/temitayo-ob/darey.io-projects/assets/145216353/882c0b80-2b25-4f08-83f5-f9aaad59a822)
other options include 
-m --- displays information in MegaBytes's
-k --- displays in kilobytes
-t --- shows file system type in a new window

### 16 du command
du command is used to check how much space a file or directory takes up.you must specify the path when using the du command.
![du command](https://github.com/temitayo-ob/darey.io-projects/assets/145216353/9ebbaa17-5e0e-4e24-bdde-29a186930e10)
other options include :
-s --- total size of a specified folder
-m --- diplays in megabytes
-k --- displays in kilobytes
-h --- informs on the last modification date 

### 17 head command 
this command views the first 10 lines of a text, adding an option allows for changing the number of lines displayed.general syntax is : 
head [option] [file]
example of its application is illustrated below
![head command](https://github.com/temitayo-ob/darey.io-projects/assets/145216353/6a3f726e-88c1-46b0-943b-a67556d067d5)
options include :
-n / -lines --- prints  first customized number of lines 
-c / -bytes --- prints first customized number of bytesof each file
-q / -quiet --- will not print headers specifying the file name 

### 18 tail command 
this command displays the last 10 lines of a file. general syntax is :
tail [option] [file]
example attached below
![tail command](https://github.com/temitayo-ob/darey.io-projects/assets/145216353/310569b3-09e3-4342-8359-b4718478f566)

### 19 diff command (difference)
The diff command compares the contents of two files line by line,It displays the parts that do not match. 
The general syntax is: diff [option] file1 file2
illustration of its usage :
![diff command](https://github.com/temitayo-ob/darey.io-projects/assets/145216353/12adbb58-4091-48e7-bd8a-788c03344aec)
options include 
-c --- displays difference in context form 
-u --- displays output without redundant information 
-i --- removes case sensitivity

### 20 tar command
tar command archives files into a tar file with optional compression. 
the general syntax is: tar [options] [archive_file] [file or directory to be archived]
![tar command](https://github.com/temitayo-ob/darey.io-projects/assets/145216353/f69e7787-d95c-4dfe-8bf8-c548f22d4a3a)

options include 
-x --- extracts a file 
-t --- lists content of a file
-u --- archives and adds to an existing archived file 

## File permissions and ownership

### 21 chmod command 
This command modifies a file or directory's read, write and execute permissions.
the syntax is : chmod [option] [permission] [file_name]
for example to allow read,write, and execute permissions for group members and others on a file :(numeric value is 777)
![chmod command](https://github.com/temitayo-ob/darey.io-projects/assets/145216353/680e847e-3392-483f-8110-ffcb5524f489)

other options include 
-c --- displays info when a change i made 
-f --- suppresses errors

### 22 chown command
This command allows changes to thye ownership of a file, directory or symbolic link to a specified username.
basic syntax is: chown [option] owner[:group] file(s)
![chown command](https://github.com/temitayo-ob/darey.io-projects/assets/145216353/d692cdef-c498-43a1-b5be-07dde21fdda0)

### 23 jobs command 
jobs are processes started from the shell, this command shows all running processes and their statuses. 
syntax :jobs [options] jobID
to check status of jobs simply type jobs. 
note -- i dont have any current jobs running so the command returns empty 
![jobs command](https://github.com/temitayo-ob/darey.io-projects/assets/145216353/3c61ffee-83f1-47a5-b040-1aa6c61ad394)

options include : 
-i --- lists process id's and information
-n --- jobs with changed statuses since last notification
-p -- process id's only.

### 24 kill command 
Used to terminate an unresponsive program manually. to kill a program the process id is used and can be gotten by typing this command in the terminal :ps ux

general syntax is : kill [signal_option] pid

signals 
sigterm --- stops programs and gives time to save its progress
sigkill --- forces programs to stop 

### 25 ping command 
Ping command is used to check if a network or server is reachable, and troubleshoot connectivity issues 
General format is: ping [option] [hostname_or_IP_address]
below is a connection check for google.com
![ping command](https://github.com/temitayo-ob/darey.io-projects/assets/145216353/9db017a7-4f84-4576-bd49-08eb76b7fe09)

### 26 wget command 
This command is used to download files from the internet, its general syntax is 
wget [option] [url]
an example to dowload wordpress is below : wget https://wordpress.org/latest.zip
![wget command](https://github.com/temitayo-ob/darey.io-projects/assets/145216353/0231f5c5-fe72-479a-8f71-6f08579f5429)

### 27 uname command 
prints information about tyour linux system and hardware like machine name, operating system and kernel. Its basic syntax is: uname [option]
options include -a(system info), -s(kernel name) and -n(system's node hostname).
![uname command](https://github.com/temitayo-ob/darey.io-projects/assets/145216353/2a9a3085-259f-4ae4-a601-046bcd6625f9)
 
### 28 top command 
Displays all running processes and a dynamic real time view of the current system. Can be used to identify and terminate a process that may use too many system recources. simply enter top into the CLI
![top command](https://github.com/temitayo-ob/darey.io-projects/assets/145216353/8c97cc9a-e6bc-4bda-87d5-0869c12bc457)

### 29 history command 
This command displays up to 500 previously executed commands allowing reuse, requires sudo priviledges to achieve this. 
syntax : history [option]
options include 
-c --- clears history
-d --- deletes entry at offset position 
-a --- appends history lines
![history command](https://github.com/temitayo-ob/darey.io-projects/assets/145216353/c5bdc853-1479-414c-b34a-9a61d62d876a)

### 30 man command 
Provies a user manual of commands or utilities u can run on the terminal, General syntax is : man [command_name]
to specify displayed section : man [option] [section_number] [command_name]
![man command](https://github.com/temitayo-ob/darey.io-projects/assets/145216353/099fac69-c2cd-4601-9537-13b890807ca3)

### 31 echo command 
Displays a line of text or string using the standard output.
basic syntax: echo [option] [string]
![echo command](https://github.com/temitayo-ob/darey.io-projects/assets/145216353/70861d43-2fbc-4783-87a6-0c4f26eb364f)

### 32 zip, unzip command 
Zip is used to compress files into a zip file, also useful for archiving files and directories. general syntax: zip [options] zipfile file1 file2….

unzip extracts files from a zip file, general syntax :unzip [option] file_name.zip
as shown in the illustration 
![zip command](https://github.com/temitayo-ob/darey.io-projects/assets/145216353/01f77677-ff11-4661-a228-586d95a1d8f5)

### 33 hostname command
This is used to show the system's hostname, General syntax is : hostname [option]
can also be used with various flags
![hostname command](https://github.com/temitayo-ob/darey.io-projects/assets/145216353/d8367044-02b9-4957-930b-6e9038769dea)
-i --- displays ip address 
-a --- displays hostname alias
-A --- displays FQDN

### 34 useradd, userdel commands


























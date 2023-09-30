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




















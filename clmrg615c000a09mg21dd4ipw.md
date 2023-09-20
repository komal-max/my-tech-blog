---
title: "Commanding Linux: Roadmap to Fundamental Terminal Skills"
seoTitle: "Commanding Linux: Roadmap to fundamental terminal skills"
datePublished: Wed Sep 20 2023 07:53:19 GMT+0000 (Coordinated Universal Time)
cuid: clmrg615c000a09mg21dd4ipw
slug: commanding-linux-roadmap-to-fundamental-terminal-skills
ogImage: https://cdn.hashnode.com/res/hashnode/image/upload/v1695196365568/f81bb7bc-97cf-47b8-83bc-078087d57a64.jpeg
tags: linux, terminal, devops, linux-for-beginners, 90daysofdevops

---

![Linux: why is it so popular with users? - Pakhotin](https://pakhotin.org/wp-content/uploads/2023/07/53113-106400-Linux-xl.jpg align="left")

Welcome to the world of Linux, where the power of your computer is just a few keystrokes away! If you're new to Linux or looking to deepen your understanding of this versatile operating system, you're in the right place. Linux's command-line interface, often referred to as the "shell," is where the real magic happens. In this blog, I'll take you on a journey through the fundamental Linux commands that every user, whether a beginner or an experienced enthusiast, should have in their toolkit. These commands will empower you to perform essential tasks, manage your system efficiently, and gain a deeper appreciation for the world of open-source computing. So, let's dive in and unlock the potential of the Linux command line!

## Linux basic commands

1. Check the present working directory.
    
    ***pwd*** command will display the absolute path of the current working directory. This is the directory that you are currently in within the terminal.
    
2. List all the files or directories including hidden files.
    
    Navigate to the directory you want to list the files and directories from using the ***cd*** command. Once you're in the desired directory, you can use the ***ls*** command with the ***\-a*** or ***\-all*** option to list all files and directories, including hidden ones.
    

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1695188344804/43149324-9480-425f-8537-eac3385e5572.png align="center")

1. Create a nested directory A/B/C/D/E
    
    To create a nested directory structure like "A/B/C/D/E" in Linux, you can use the ***mkdir*** command with the ***\-p*** option. This option allows us to create multiple directories at once, even if the parent directories don't exist.
    
    To see or print the directories you have created in the nested structure "A/B/C/D/E," you can use the ***tree*** command if it's installed on your system. If not installed already, use ***apt-get install tree*** and check its version to see if it has been installed properly using ***tree --version.***
    

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1695189144832/a1202177-cde6-43c1-85c4-f2289d4dea11.png align="center")

1. Create a new file and display what's written in a file.
    
    To create a new file use ***touch &lt;filename&gt;*.**
    
    To edit the file use ***vim &lt;filename&gt;.*** Make sure you have an editor, to check that use ***which vim*** and if it returns an output, that means you have the editor installed (you can also use nano). If no output received, then simply install the editor using commands: ***apt-get update*** and ***apt-get install vim.*** After installing, do ***vim &lt;filename&gt;*** to edit the file and then use ***'i'*** to insert the text and when done press escape and type ***wq!*** to save the contents of the file.
    
    To view the contents of the file use ***cat &lt;filename&gt;.***
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1695190282387/5a3cadc5-557c-43c2-9eed-d1c1befd36d1.png align="center")
    
2. Check the current permissions of a file.
    
    You can view the current permissions of a file in Linux using the ***ls*** command with the ***\-l*** (long format) option. This command will display detailed information about the file, including its permissions.
    

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1695190784937/3453709c-1ab4-4ae5-9d1b-0ad41f5a0263.png align="center")

The set of permissions ***-rw-r--r--*** means the following:

* The first character, ***\-*** indicates that it's a regular file (not a directory or a special file).
    
* The next three characters, ***rw-***, represent the permissions for the file's owner (in this case, the owner is "root"):
    
    ***r*** : Read permission, meaning the owner can read the file.
    
    ***w*** : Write permission, meaning the owner can modify or delete the file.
    
    ***\-*** : No execute permission, meaning the owner cannot execute the file as a script or program.
    
* The following three characters, ***r--*** , represent the permissions for the file's group (in this case, the group is also "root"):
    
    ***r*** : Read permission, meaning members of the group can read the file.
    
    ***\-*** : No write permission, meaning members of the group cannot modify or delete the file.
    
    ***\-*** : No execute permission, meaning members of the group cannot execute the file.
    
* The last three characters, ***r--*** , represent the permissions for others (all users who are not the owner and not in the group "root"):
    
    ***r*** : Read permission, meaning others can read the file.
    
    ***\-*** : No write permission, meaning others cannot modify or delete the file.
    
    ***\-*** : No execute permission, meaning others cannot execute the file.
    

In summary, the file is readable by everyone (owner, group, and others), but only the owner can modify it. None of them can execute it as a script or program because there is no execute permission set.

To understand linux file permissions properly, you can check this guide below: [https://www.linuxfoundation.org/blog/blog/classic-sysadmin-understanding-linux-file-permissions](https://www.linuxfoundation.org/blog/blog/classic-sysadmin-understanding-linux-file-permissions).

Another example of file permissions is as below which I found to be very helpful (source- linux training academy)

![](https://www.linuxtrainingacademy.com/wp-content/uploads/2017/02/linux-permissions-chart.png align="left")

The table below is very useful when it comes to understanding file permissions in depth. (source- [https://www.multacom.com/faq/password\_protection/file\_permissions.htm](https://www.multacom.com/faq/password_protection/file_permissions.htm))

Permissions consist of three numbers: 4 for Read, 2 for Write, and 1 for Execute access. By adding these numbers together, you form the permissions that make up one digit.

<table><tbody><tr><td colspan="1" rowspan="1"><p><strong>&nbsp;</strong></p></td><td colspan="1" rowspan="1"><p><strong>User</strong></p></td><td colspan="1" rowspan="1"><p><strong>Group</strong></p></td><td colspan="1" rowspan="1"><p><strong>Other</strong></p></td></tr><tr><td colspan="1" rowspan="1"><p><strong>Read = 4</strong></p></td><td colspan="1" rowspan="1"><p>x</p></td><td colspan="1" rowspan="1"><p>x</p></td><td colspan="1" rowspan="1"><p>x</p></td></tr><tr><td colspan="1" rowspan="1"><p><strong>Write = 2</strong></p></td><td colspan="1" rowspan="1"><p>x</p></td><td colspan="1" rowspan="1"><p>&nbsp;</p></td><td colspan="1" rowspan="1"><p>&nbsp;</p></td></tr><tr><td colspan="1" rowspan="1"><p><strong>Execute = 1</strong></p></td><td colspan="1" rowspan="1"><p>x</p></td><td colspan="1" rowspan="1"><p>x</p></td><td colspan="1" rowspan="1"><p>x</p></td></tr><tr><td colspan="1" rowspan="1"><p><strong>Totals</strong></p></td><td colspan="1" rowspan="1"><p>(4+2+1) = 7</p></td><td colspan="1" rowspan="1"><p>(4 + 1) = 5</p></td><td colspan="1" rowspan="1"><p>(4 + 1) = 5</p></td></tr></tbody></table>

For example, 4 + 2 + 1 = 7, which grants read, write, and execute permissions; 4 + 1 = 5, which grants only read and execute permissions. Thus, 755 grants 7 (read, write, execute) to the owner of the file, and 5 (read and execute) to the group the file is in and 5 (read and execute) to the world. Each digit corresponds to a set of permissions (read, write, or execute) and the position of the digit corresponds to the user category (left = owner, middle = group, right = other). The single-digit numbers are defined for all three user categories as the following:

<table><tbody><tr><td colspan="1" rowspan="1"><p>0</p></td><td colspan="1" rowspan="1"><p>&nbsp;- - -</p></td><td colspan="1" rowspan="1"><p>no access</p></td></tr><tr><td colspan="1" rowspan="1"><p>1</p></td><td colspan="1" rowspan="1"><p>- - x</p></td><td colspan="1" rowspan="1"><p>execute only &nbsp;</p></td></tr><tr><td colspan="1" rowspan="1"><p>2</p></td><td colspan="1" rowspan="1"><p>- w -</p></td><td colspan="1" rowspan="1"><p>write access only</p></td></tr><tr><td colspan="1" rowspan="1"><p>3</p></td><td colspan="1" rowspan="1"><p>- w x</p></td><td colspan="1" rowspan="1"><p>write and execute</p></td></tr><tr><td colspan="1" rowspan="1"><p>4</p></td><td colspan="1" rowspan="1"><p>r - -</p></td><td colspan="1" rowspan="1"><p>read only</p></td></tr><tr><td colspan="1" rowspan="1"><p>5</p></td><td colspan="1" rowspan="1"><p>r - x</p></td><td colspan="1" rowspan="1"><p>read and execute</p></td></tr><tr><td colspan="1" rowspan="1"><p>6</p></td><td colspan="1" rowspan="1"><p>r w -</p></td><td colspan="1" rowspan="1"><p>read and write</p></td></tr><tr><td colspan="1" rowspan="1"><p>7</p></td><td colspan="1" rowspan="1"><p>r w x</p></td><td colspan="1" rowspan="1"><p>read, write and execute (full access)</p></td></tr></tbody></table>

1. Change the permission of a file
    
    To change the permissions of a file, for example- grant read, write and execute permissions to user, read, write to group and only read to others, use:
    
    ***chmod 764 &lt;filename&gt;***
    

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1695192327559/689517b6-44db-4be8-96ac-8dc63cf584a0.png align="center")

1. Check which commands you have run till now.
    
    you can use the ***history*** command to view a list of commands that you have run in your current shell session. By default, it displays a numbered list of previously executed commands along with their respective command numbers.
    
    To recall and re-run a specific command from your command history, you can use the ***!*** (exclamation mark) followed by the command number.
    

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1695192661492/e415369d-034f-4544-a3ec-4939f6eb542b.png align="center")

1. Remove a directory/folder.
    
    To remove a directory in Linux, you can use the ***rmdir*** or ***rm*** command, depending on whether the directory is empty or contains files and subdirectories.
    
    If the directory is empty, you can use the ***rmdir &lt;directory name&gt;***.
    
    To remove a directory and its contents, including files and subdirectories, you can use the ***rm*** command with the ***-r*** (recursive) option, ***rm -r &lt;directory name&gt;.***
    
    In some cases, you may need to force the removal of a directory and its contents, even if some files are write-protected or if there are permissions issues. You can use: ***rm -rf &lt;directory\_name&gt;***
    

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1695193694687/17a514fb-0201-4785-a6a2-d6481917d6e6.png align="center")

1. Output only the top/bottom n number of lines from the file.
    
    To display only specific number of lines from the top/bottom of a file in Linux, you can use the ***head/tail*** command with the ***\-n*** option, specifying the number of lines you want to display.
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1695194121420/5b83b1ca-75e3-4aef-abd4-9b302c8c0972.png align="center")
    
    1. View currently running processes.
        
        To view the currently running processes in Linux, you can use the ***ps*** command.
        
        ***ps*** : This will display a list of processes running in your current terminal session.
        
        **ps -e** : This will display a list of all processes running on the system, including those belonging to other users.
        
        ***ps -aux*** : This will show a detailed list of processes for the current user, including process IDs (PIDs), CPU usage, memory usage, and more.
        

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1695194881602/9e24209a-a8d0-433f-9044-b399f5db71d2.png align="center")

1. Check disk space utilization.
    
    The ***df*** command provides information about disk space utilization on your system. By default, it displays information about all mounted filesystems. The ***\-h*** option is used to display sizes in a human-readable format (e.g., in gigabytes and megabytes) rather than in blocks.
    
    The ***du*** command is used to estimate and display the space used by files and directories. The ***\-s*** option is used to summarize the space used by the specified directory, and ***-h*** provides human-readable sizes.
    

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1695195408724/214681de-965b-4f33-8f1f-07fd78a6cb23.png align="center")

1. Check CPU utilization.
    
    The ***top*** command provides real-time information about CPU usage, memory usage, and other system statistics. It will display a dynamic list of processes along with CPU utilization details. You can press the ***q*** key to quit when you're done.
    

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1695195640971/95e6ab4b-9314-4ec1-a617-6b159fff9181.png align="center")

In this blog, we've only scratched the surface. The commands and concepts we've covered here are just the beginning of a vast and exciting journey. Linux is a versatile and powerful operating system, and there's always something new to discover. Whether you're delving into advanced command-line tools, exploring system administration, or diving into the world of open-source development, remember that learning is a lifelong adventure. So, keep exploring, keep experimenting, and keep expanding your Linux knowledge. There's still so much more to learn, and the possibilities are endless.

Happy Learning!

## References

1. [https://www.linuxfoundation.org/blog/blog/classic-sysadmin-understanding-linux-file-permissions](https://www.linuxfoundation.org/blog/blog/classic-sysadmin-understanding-linux-file-permissions)
    
2. [https://hub.docker.com/\_/ubuntu](https://hub.docker.com/_/ubuntu)
    
3. [https://www.multacom.com/faq/password\_protection/file\_permissions.htm](https://www.multacom.com/faq/password_protection/file_permissions.htm)
    
4. [https://www.linuxtrainingacademy.com/](https://www.linuxtrainingacademy.com/)
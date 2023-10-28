# LinuxPracticeProject
## A repo that documents my learning path in Linux
### PART 1: File Manipulation
1. sudo command: performing tasks that require administrative or root permissions. e.g;
  - sudo apt upgrade, 
  ![image](./images/sudo_apt_upgrade.png)
  - sudo -h, 
  ![image](./images/sudo-h.png)

2. pwd command: returns full current path in the working directory
  ![image](./images/pwd.png)

3. cd command: switches to a completely new directory
  ![image](./images/cd.png)

4. ls command: lists files and directories within a system
  ![image](./images/ls.png)

5. cat command: lists, combines and writes file content. e.g;
  - cat filename.txt - lists or writes contents in a file from the top
  ![image](./images/cat.png)
  - tac filename.txt - lists or writes contents in a file from the bottom
  ![image](./images/tac.png)

6. cp command: copy files or directories and their content
  ![image](./images/cp.png)

7. mv command: move and rename files and directories
  - moving a file
  ![image](./images/move.png)
  - renaming a file
  ![image](./images/rename.png)

8. mkdir command: creates one or multiple directories and sets permissions for them
  - creating a new directory
  ![image](./images/mkdir.png)
  - creating a new directory inside an existing directory
  ![image](./images/mkdir_inside.png)
  - creating a directory between two existing folders 
  ![image](./images/mkdir_between.png)
  - creating a directory with full read, write and execute permissions for all users
  ![image](./images/mkdir_with_full_access.png)

9. rmdir command: permanently deletes an empty directory (must have sudo privileges in the parent directory)
   ![image](./images/rmdir.png)

10. rm command: delete files within a directory (if there are 'write permissions')
    ![image](./images/rm_within.png)
    NB: -i prompts system confirmation before deleting a file; -f removes without confirmation, -r deletes files and directories recursively
    ![image](./images/rm_with_notice.png)

11. touch command: creates an empty file
    ![image](./images/touch.png)

12. locate command: finds a file in the database system.
    NB: 1. adding -i turns off the case sensitivity; 2. add an * if looking for 2 or more words
    ![image](./images/locate.png)

13. find command: search for files within a specific directory and performs subsequent operations. 
    ![image](./images/find.png)
  or
    ![image](./images/find2.png)

14. grep command: allows search for a word through all the texts in a specific file
    ![image](./images/grep.png)

15. df command: reports the system's disk space usage displayed in % and kb
    - df -h
    ![image](./images/df-h.png)
    - df -m
    ![image](./images/df-m.png)
    - df -k
    ![image](./images/df-k.png)
    - df -T
    ![image](./images/df-T.png)

16. du command: checks how much space a directory or file takes
    ![image](./images/du.png)
    1. -s gives the total size of a folder; 
    ![image](./images/du-s.png)
    2. -m gives folder and file information in MB;
    ![image](./images/du-m.png)
    3. -k gives folder and file information in KB;
    ![image](./images/du-k.png)

17. head command: views the first 5 lines of a text
    ![image](./images/head.png)

18. tail command: views the last 5 lines of a text
    ![image](./images/tail.png)

19. diff command: compares two contents of a file line by line and displays the unmatched part
    ![image](./images/diff.png)
    1. -c displays the difference between two files contextually 
    ![image](./images/diff-c.png)
    2. -u displays the output without redundant information
    ![image](./images/diff-u.png)
    3. -i makes the diff command case insensitive
    ![image](./images/diff-i.png)

20. tar command: archives multiple files into a TAR file 
    ![image](./images/tar.png)

------------------------------------------------------------------------------------------------------------------------

### PART 2: File Permissions and Ownership

21. chmod command: modifies a file or directory's read, write and execute permissions
    ![image](./images/chmod.png)

22. chown command: changes the ownership of a file, directory or symbolic link to a specified username
    ![image](./images/chmod2.png)


23. jobs command: This is a process started by the shell. It displays all running processes along with their statuses
  NB: available only in csh, bash, tcsh, and ksh shells
    - -i list process IDs and their info;
    - -n lists jobs whose status have changed since last time;
    - -p list process IDs only

24. kill command: terminates an unresponsive program manually. To do this, one must know the PID - process identification number. ie. ps ux
    ![image](./images/kill.png)
after getting the signal and PID, enter 'kill [signal_option] pid'
e.g. of signals = SIGTERM, SIGKILL

25. ping command: checks whether a network or a server is reachable. used for troubleshooting connectivity issues
    ![image](./images/ping.png)

26. wget command: lets you download files from the internet. It works in the background without hindering other running processes
    ![image](./images/wget.png)

27. uname command: prints detailed info about your Linux system and hardware
    ![image](./images/uname.png)
  Other options include:
  - -a which prints the system info; -s prints the kernel name; and -n prints the system's node hostname
    ![image](./images/uname-a.png)

28. top command: displays all the running processes and real-time view of the current system. Also helps to identify and terminate a process that may use too many resources
    ![image](./images/top.png)

29. history command: lists up to 500 previously executed commands, and allows you to reuse them without re-entering. Only user with sudo privileges can perform it
    ![image](./images/history.png)

30. man command: provides manual of any commands or utilities run in the terminal
    man ls = ![image](./images/man.png)

31. echo command: displays a line of text or string using the standard output
    OPTIONS: -n displays output; -e enables interpretation of backlash escapes; \a plays sound alert; \b removes spaces in between the text; \c produces no further output; -E displays the default option and disables the interpretation of backlash escapes.

32. zip, unzip commands: compresses files into ZIP file
    ![image](./images/zip_unzip.png)

33. hostname command: reveals the system's hostname
    - can be executed alone or with other flags like -a, -i, -d
    ![image](./images/hostname.png)

34. useradd, userdel commands: 
    - useradd: creates new account
    ![image](./images/useradd.png)
    - userdel: deletes account
    ![image](./images/userdel.png)

35. apt-get command: retrieves information and bundles from authenticated sources to manage software
    - apt-get update: synchronizes the package file from their sources
      ![image](./images/apt-get1.png)
    - apt-get upgrade: installs the latest version of all installed packages
      ![image](./images/apt-get2.png)
    - apt-get check: updates the package cache and checks broken dependencies 
      ![image](./images/apt-get3.png)

36. nano, vi, jed commands: allows user to edit and manage files via a text editor
    - nano: using command nano DevOps to read the file named DevOps
      ![image](./images/nano.png)
    - vi: saves, opens, copies, and pastes a file
      ![image](./images/vi.png)
    - jed: has modes to load modules or plug-ins to write specific text
      ![image](./images/jed.png)

37. alias, unalias commands: alias allows you to create shortcut with the same functionality as a command, file name or text, while unalias deletes an existing alias
      ![image](./images/alias.png)

38. su command: switch user command allows you to run a program as a different user
    NB: other options include, -p, -s, -l
    ![image](./images/su.png)

39. htop command: monitors system resources and server processes in real-time
    ![image](./images/htop.png)
    - -C or -no-color: enables the monochrome mode
      ![image](./images/htop-C.png)
    - -help: displays the help message and exit
      ![image](./images/htop-help.png)

40. ps command: is called process status command, it produces a snapshot of all the running processes in the system
    ![image](./images/ps.png)
    - -T displays all the processes associated with the current shell session
      ![image](./images/ps-T.png)
    - -u list processes associated with a specific user
      ![image](./images/ps-u.png)
    - -A or -e shows all the running processes
      ![image](./images/ps-A.png)


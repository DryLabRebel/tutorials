File Permissions Cheat Sheet
============================

---

    Author: Geoff
    Last Update: (13-03-2025)

---

Preamble
--------

This is a quick tutorial/reference for getting familiar with the terminal, and the Bourne-Again (bash) 'shell'.

NOTE: I often provide examples of code to type. Whenever you see the `$` dollarsign, this indicates that the code is to be typed on the command line. The `$` is itself representative of the shell prompt. So you wouldn't type it in, but you would type what follows exactly as indicated in the example.

---

Context
-------

If you run the command `ls -l` in a terminal, it would output the files and directories, in the current directory, including some useful information about them. For example:

    -rwx------ 1 <owner> <group> 9834 Mar 20 17:47 file1.txt
    -rw-r-x--- 1 <owner> <group> 7922 Mar 22 08:52 file2.txt
    -rwxr-x--- 1 <owner> <group> 7922 Mar 22 08:52 file3.txt
    -rwsr-x--- 1 <owner> <group> 7922 Mar 22 08:52 file4.txt

Permissions
-----------

Bit permissions are indicated by the little letters and dashes at the beginning of the line. They indicate a files 'read', 'write' and 'execute' permission for the file owner, the group, and 'all' (anyone else).

### Permission Types ###

Here is a list of the most common things you will find for any given file:

`-` -- No permission  
`r` -- Read  
`w` -- Write  
`x` -- Execute  
`s` -- kind of like Execute, but not exactly the same! (close enough that you generally don't need to worry about it)  
`S` -- Most of the time, this means a directory is not executable, but it probably should be (because it contains files which are executable/accessible)

\pagebreak

### File Permission Layout ###

Here is a simple schematic with several examples of possible configurations:

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;file/directory *owner*  
&nbsp;&nbsp;&nbsp;`/`  
&nbsp;&nbsp;&nbsp;`|`  &nbsp;&nbsp;&nbsp;&nbsp;*group* members  
&nbsp;&nbsp;&nbsp;`|  /`  
&nbsp;&nbsp;&nbsp;`|  |`  &nbsp;&nbsp;&nbsp;&nbsp;any other *users*  
&nbsp;&nbsp;&nbsp;`|  |  /`  
&nbsp;&nbsp;&nbsp;`|  |  |`  
&nbsp;&nbsp;&nbsp;`|  |  |`  
&nbsp;&nbsp;`/ \/ \/ \`  
&nbsp;&nbsp;`| || || |`  
`drwxrwsrwx`  
`drwxrws---`  
`drwxrwsrwx`  
`-rw-rw----`  
`drwxrws---`  
`drwxrwxrwx`  
`-rw-rw----`  
`drwxrwxrwx`  
`drwxrws---`  
`drwxrwsrwx`  
`drwxrwsrwx`  

### `chmod` file permission codes ###

`0 == -`  
`1 == x`  
`2 == w`  
`3 == wx`  
`4 == r`  
`5 == rx`  
`6 == rw`  
`7 == rwx`  

Usage
-----

### Command Syntax ###

Type something like this on the command line (in your terminal):

    $ chmod 770 <path/filename>

### Examples ###

    $ chmod 700 ./* 

The above command will provide read, write, execute permission for all files (`*` is a wildcard for 'anything') in the current directory, for the file owner, but no permissions for anyone else (group or users).

    $ chmod 660 some_directory/ 

The above command will provide read and write, but *not* execute permission for `some_directory/`, for the *file owner and the group members*, but no permissions for any other users.

    $ chmod 760 some_directory/some_file.txt

The above command will provide read, write and execute permission for `some_file.txt` for the file owner, and also read, write, but *not execute* permissions for group members, but no permissions for any other users.

For more information on file permissions type in `man chmod` into your terminal command line.

Another Way
-----------

There is another way to use `chmod` which involves explicitly setting the read, write or execute permissions.

Stay tuned...


Memory/CPU management
=====================

---

    Author: Geoff English
    Last Update: (13-03-2025)

---

Preamble
--------

A brief tutorial on healthy memory management habits when using the AQZ team machines.

This tutorial assumes you are already familiar with htop (and at least know how to access the terminal and type commands into it).

If you haven't familiarised yourself with htop, then you should definitely go and check out my very short tutorial on htop which can be found [here](htop.md).

Just briefly, htop is a user friendly display/overview of system processes which is highly configuarble, and has options for managing processes also, including the ability to end (kill) them.


NOTE: I often provide examples of code to type. Whenever you see the `$` dollarsign, this indicates that the code is to be typed on the command line. The `$` is itself representative of the shell prompt. So you wouldn't type it in, but you would type what follows exactly as indicated in the example.

---

Preamble
--------

> Prevention is better than cure.

The AQZ machines are a specific subset of machines, specifically allocated to the CEGS | Performance and Tax Gap crew. The hardware is owned/managed by DXC, and the Operating systems and system
adminstration is run by EST.

We are not a part of Smarter Data, and therefore we do not use the AAP platform, and our access to the Data Warehouse is heavily regulated and restricted.

As a team, our workflow typically revolves around the running of large statistical models, many of which operate over very large datasets, which require a significant amount of storage, processing power, and use of available RAM memory.

Also, the majority of the models we run are run in `python` or `R` which (generally) require data to be loaded into (RAM) memory.

Therefore the most significant considerations with the regular use of the AQZ machines is how to effectively and efficiently manage the use of the available storage, RAM memory and processing power of the machines.

So this tutorial will be providing a very short and practical overview of ways to more efficiently manage the use of the limited memory and processing power of the AQZ machines.

---

Good Memory/Process Management Habits
-------------------------------------

### Taking out the Trash ###

In this section we'll talk about how to keep your trash empty, which is not as straightforward as it sounds. For starters, there is more than one trash bin...

#### Multiple trash cans ####

On the AQZ machines, we have a dedicated storage space which can be accessed by the `/nfsXXX/` directory, where 'XXX' corresponds to the team machine you're using (075, 003, etc.).

This is a separate space from the team machines themselves. However, as you probably already know, you can access this directory from the team machines as you would any other directory. That is, you can access it using the files app, or by using the terminal.

**However**, because it's a separate space from the team machines, if you use the files app to delete files/directories there *they* ***DO NOT*** *get moved to the 'trash' on the team machine* (i.e. the directory that you access using the 'trash' icon in files).

If you typically use the files app on the AQZ machines to delete anything (including large datasets) on the nfs storage space, they get moved to a *different* trash directory, which is on the nfs space itself. Furthermore, each user has their own unique trash directory.

Crucially, this means files and directories don't get deleted but *moved* to the respective trash directory, not actually clearing any space on the nfs drives. Also, simply 'emptying' your trash on the team machine, won't empty the nfs storage trash directory.

So the next section will explain how to clear your nfs trash.

#### No Prisoners ####

To permanently delete anything in the nfs shares, you must use the terminal.

Open up a terminal (Click 'Activities' in the top right of your screen, then click the terminal icon in the pop up menu), then navigate to nfs using this command:

    $ cd /nfsXXX/ 
    
where XXX is the team machine you are using (075, 078, etc.).

Then type: 

    $ ls -la 
    
to list the contents, including hidden files and details (The `l` and `a` in `-la` tell the terminal to list the details of each file, and to include hidden files/directories in the list respectively). 

What you will see (among other things), is a bunch of `.Trash-xxxxxxxx` directories where `xxxxxxx` corresponds to your user-id (UID), which is assigned to you by the team machine. It's not your username.

You should also see the usernames associated with each trash directory. It should look something like this:

    drwxrwS---   4 <username>@prod.atonet.gov.au res_AQZlfb_bslnfs075_rw_gg@prod.atonet.gov.au   43 Apr 24 14:57 .Trash-xxxxxxxxx

The trash directory has two directories inside it: 'files' and 'info'. Both of these directories need to be emptied.

If you find one that belongs to you then use:

    $ cd .Trash-xxxxxx
    
to navigate into it.

Now you need to empty the 'files' and 'info' directories.

Do this with the `rm` command.

From here type:

    $ rm -rf info/* files/* 

to remove the contents permanently (***This cannot be undone!!***).

The `r` and `f` in `-rf` tell the terminal to, respectively, delete 'recursively' (include directories), and also to omit the confirmation request in the operation (I.e. don't ask me... just do it!). Use the `-f` flag ***with extreme caution***.

To confirm that these directories are now empty you can run:

    $ ls -la info/ files/

And you should see:

    total 0

Or something to that effect, indicating that there are zero contents to list.

Users should do this on any machine for which they have access or have used ever.

*In case you missed it, this will* ***PERMANENTLY*** *delete* ***everything*** *in the* `files/` *and* `info/` *directories, inside your trash directory! (and if it's not mind numbingly obvious will also do the same thing if you use this command anywhere else on the team machines, so be very careful if you decide to start indiscriminantly deleting stuff using* `rm -rf`*!!!)*

Simple? Simple...

Finally, note also that you only have permission to access your own Trash, so no one else can empty it for you and vice versa.

### Prevention is Better than Cure ###

Under the pretence that we want to keep the nfs shares as clean and tidy as possible, obviously it would be preferrable to ensure that data is permanently deleted in the first place. There are at least two ways to do this.

#### Removing files using the terminal ####

The first method is to use the terminal (as described above) to remove unwanted files and data. The standard method for removing files using the terminal is by using the remove command like so:

    $ rm stuff/to/remove.file

The machines are setup to prompt you for confirmation whenever you attempt to remove any file, which is a good thing.

    rm: remove regular empty file ‘tester’? 

To which you respond either `y` or `n`, and press enter. As already stated the `rm` command *permanently removes* whatever you tell it to remove (assuming you have permission to do so).

This is the most straightforward and simplest way to ensure that when you remove data, it is gone[^rmnote].

There are a few extra options that are most common and useful when removing files using the `rm` command on the terminal: 

- `-f`
- `-i`
- `-r`

We've already seen the flag `-f` which means 'force'. In other words you don't want confirmation. You know what you're doing and you just want to remove the file. This is a very handy flag when you have any more than 2 or 3 files you aim to remove at once, as it will ask you for confirmation each time.

The real utility of requiring confirmation is questionable, in my opinion. If I'm in a hurry to delete a file, and I make a mistake, then I'm probably going to confirm in a hurry also and/or include this extra `-f` flag which is very easy to do.

A ***much*** safer habit to practise, is to test your syntax before you remove anything. So, before you ever delete any files, run the list command first, to see exactly what is going to be deleted!:

    ls stuff/to/remove.file

This is **especially** useful when deleting multiple files using some kind of wildcard syntax. For example, if I wanted to delete a bunch of log files, then I would do something like this:

    ls path/to/log/files/*

This command will show me all files in the `path/to/log/files/` directory. Maybe I only want to delete files ending in 'log', so then I would run:

    rm -f path/to/log/files/*.log

Anyway, you get the idea.

The second flag is `-i`, which is the exact opposite of `-f`. When confirmation is not enabled by default, you can use the `-i` flag to ensure that `rm` asks for confirmation before deleting anything.

Finally `-r` means 'recursive'. This flag is necessary for deleting directories. This tells `rm` that you want to delete a directory, and all of its contents. Without this flag you'll get an error:

    rm: cannot remove ‘foobar/’: Is a directory

Hopefully you can immediately see how potentially dangerous the combination of `rm -rf` might be. So that's all I'm going to say about that.

#### Removing files using WinSCP ####

Shout out to Matthew Wilson for pointing out a second option, which is that you can also use winSCP to delete files on the `/nfsXXX/` directory.

Given that winSCP is run on the windows side of the connection, it is not configured/able to use the trash directory on the UNIX machines, and thus anything deleted using winSCP will be permanently deleted.

This is obviously a much more user friendly way of deleting files, which is a huge advantage. The main drawback here is that it requires logging into winSCP.

---

htop
----

When it comes to memory management, `htop` is one of the most important tools at your disposal. `htop` is a very useful program which provides visually simple, real time updates on system processes and memory usage.

The best way to access htop is from a terminal.

Besides being a very convenient method for viewing the memory usage and processes on the team machines, htop can also manipulate processes, including killing them.

At the bottom of the display htop tells you that you can kill processes by pressing <kbd>F9</kbd>. The equivalent to this is to press <kbd>k</kbd> (short for kill, in case that's not mind numbingly obvious!).

What both of these keys actually do is open a signal menu, which allows you to select a signal, from a list, to send to the selected process/es.

If you want to kill a selected process, the signal is `SIGTERM`, which you can select by navigating to it using your arrows keys, and pressing <kbd>Enter</kbd>. Alternatively, you can simply select the process number (indicated on the left of the signal name), which for `SIGTERM` is 15, and this will send the signal directly.

One handy feature which I did not cover in the [htop](htop.md) tutorial, is the ability to select multiple processes at once by tagging them, which can be done by navigating through the process list, and then selecting the highlighted process by pressing <kbd>SpaceBar</kbd>.

In this way you can (somewhat laboriously) work your way through the process list, and manually selecting any/all processes you like, to perform bulk operations on them. Sending a signal to a selected list is the same as for a single process.

If you have a (relatively short) list of processes, say a few ghost processes that need to be destroyed, this is a really convienent and user friendly way to work through and select them, then kill them all in bulk.

It's great because you can see the process command, and be sure you're selecting the one you want.

NOTE: Killing a parent process will *not necessarily* kill the child processes for that process. A more robust method is to use tree mode (press <kbd>t</kbd>), and select a process, and all child process (using <kbd>Space</kbd>) of that process, and killing them all explicitly.

This is a particularly useful method for finding and destroying ghost processes on your team machine.

For most of the situations where users of the AQZ machines will need to kill a process, doing so manually in htop is going to be the simplest and best.

For more information on navigating htop (including how to slow down the refresh rate, which is particularly useful for viewing/selecting processes) see the tutorial: [htop](htop.md)

pkill
-----

`pkill`, and it's sister command `pgrep`, are programs dedicated to interacting with the process list on your machine.

`pgrep` (much like 'grep') lists processes, and can be told to list processe which match an input.

`pkill` can be used to send process signals to a specified process including, especially process termination signals (such as `SIGKILL`, for example). 

The default signal used by `pkill` is `SIGTERM`

### Signals ###

Wait, what are signals?

Good question. Signals are a way of sending commands to system processes on UNIX machines (like the team machines!). There are many signals.

If you're interested, you can list the available signals your machine with the following command:

    $ trap -l

Also `trap` is a program for trapping (and listing evidently) signals.

For the purposes of ending processes, the two most important signals are:

- `SIGTERM` - This signal attempts to shut down a process somewhat intelligently. It will attempt to kill off child processes and shut the process down, it can also be blocked by a process if the process has measures in place to prevent its own termination
- `SIGKILL` - Kill the process. This is the golden gun of kill commands. No exceptions, no mercy. This will kill a process, and there's nothing that process can do to stop it.
[//]: # ( So, in terms of handling processes, there are signals, which can kill them, which can be handled using htop, pkill, kill, and likely other programs also )

Disk Usage Analyser
-------------------

---

Footnotes
---------

[^rmnote]: Fun-fact!, your machine never immediately 'removes' data, even when you 'permanently' delete it (AFAIK this is true of most computers). What it really does is simply mark the space on the harddisk as being available to be written to, so that new data may eventually overwrite the existing space. NOTE: it is ***extremely difficult*** to retrieve this data once it has been 'removed', so do not rely on this as any kind of safeguard or backup option if you accidentally delete something!

---

ToDo:

[//]: # ( Email regarding Data curation! Include a section in here about keeping track of datasets, and ensuring unecessary data is not taking up space! )

[//]: # ( 
- good memory management
  - keep stock of data
  - being aware of the data inventory spreadsheet
  - file compression
    - gzip
    - rds
    - tar
  - regularly emptying trash bins
  - avoiding the use of 'files' to delete data/files in the nfs shares
  - nfs is for data, home is for code
  - disk usage analyser

- good process management
  - generally covered by the above
    - logging in/out properly
    - loggin out of the machines
    - keeping track of process usage
    - optimising code for efficiency
  - manually handling/killing processes
    - generally only when necessary
    - signals
    - htop
    - kill/pkill
    - ps
)

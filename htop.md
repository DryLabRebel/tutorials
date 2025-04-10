Htop
====

---

    Author: Geoff English
    Last Update: (13-03-2025)

---

Preamble
--------

This is a quick tutorial and reference for how to best understand what you see in htop.

Htop is a user friendly, terminal application that provides users with a quick overview of system processes. 

It is also relatively configurable, and provides several options to help you manage your processes.

NOTE: I often provide examples of code to type. Whenever you see the `$` dollarsign, this indicates that the code is to be typed on the command line. The `$` is itself representative of the shell prompt. So you wouldn't type it in, but you would type what follows exactly as indicated in the example.

---

getting into htop
-----------------

Assuming htop is installed on your machine, it can be accessed by opening up a terminal session.

At the prompt simply type:

    $ htop 
    
and press enter.

From here, you should be greeted with a screen that looks something like this:

[Image] -- Stay tuned

---

Boring, but brief overview
--------------------------

The top section displays bars showing the relative usage of the processor cores, and some other summary information, most notable the Mem and Swp bars.

Most of this information is *relatively* self explanatory. The numbers represent the core the bar is associated with (in other words, the number of bars you see should correspond to the number of processor cores on your machine), and The Mem and Swp bars which represent the Memory usage and Swap usage bars respectively.

`Mem` -- represents the amount of physical RAM (Random Access Memory) currently in use.  
`Swp` -- is... for all intents and purposes... Memory in the physical harddrive, that is being 'borrowed' and used as RAM.

In general, when Memory usage exceeds capacity, processes start using Swp as a 'spillover'. The machines try to be fairly smart about this, and try to use Swp for things that are mostly inactive, or don't require high performance. However it's not perfect, and having your machine using excessive Swp does tend to lead to reduced performance.

---

The bottom section displays a list of running processes. At the top you'll see a header containing column names such as PID, USER, VIRT, CPU%, COMMAND, and more (which conveniently doubles as a neat visual separator between the top and the bottom section of the htop display!)

Generally, the most useful columns are:

- `PID`     -- The process ID
- `USER`    -- The username of the process owner
- `RES`     -- The amount of physical memory being used by the process (Mem is, *pretty much*, a sum of the RES of each process)
- `MEM%`    -- The percentage of physical memory allocated to this process
- `CPU%`    -- The percentage of CPU power currently being used by this process (at the moment in time that the display was updated)
- `TIME+`   -- The amount of compute time that has elapsed since this process was invoked (if the process is idle, then it's `TIME+` will not increase)
- `COMMAND` -- The process name, or terminal command that created this process

Another interesting column is `VIRT`, which displays the amount of 'potential' memory, that can be used by the process... the amount that it has been allocated, if it should so like to use it! I included this mostly so that you don't confuse it with `RES`. `RES` is how much memory is actually in use, `VIRT` is how much any given process can *potentially* use.

---

Navigation
----------

The first and simplest navigation tool is to simply use the up and down arrows to scroll through the process list, but there are many more useful ways.

Firstly, you can also quickly jump to the top or bottom of the process list using the <kbd>Home</kbd> and <kbd>End</kbd> keys.

Secondly, you can scroll up and down through the process list, one page at a time using <kbd>PgUp</kbd> and <kbd>PgDn</kbd> keys.

But there are *still* more...

### Columns ###

You can quickly sort the processes by any column by simply hovering your cursor over the column header with your mouse, and clicking the column name of interest.  This is one of the simplest and most useful ways to scrutinize the process list.

You can also select the sort column by adding the sort flag, `-s` (OR `--sort-key`), when you first start htop from the command line, like so:

    $ htop -s <columnname>

OR this:

    $ htop --sort-key <columnname>

For example:

    $ htop -s PID

will start htop, and process list will be sorted by the process ID number column.

### Tree mode ###

Another very useful tool is tree mode. Tree mode displays the processes using a very aesthetically pleasing tree structure indicating the process heirarchy.

That is to say, each process can be easily traced back to its parent process.

You can access tree mode in two ways.

The first is to simply press <kbd>t</kbd> while in htop.

Secondly, you can start htop in tree mode by adding the 'tree' flag, `-t` (OR `--tree`), when you start htop in a terminal. So you would open up a terminal, and type this:

    $ htop -t

OR this:

    $ htop --tree

to start htop in tree mode.

NOTE: If you sort the processes by any column this will disable tree mode. Similarly, using tree mode disables the sorting by any other column (which makes perfect sense honestly).

### Delay ###

The delay setting, is not a navigation setting per-se. However it's helpful for navigating your process list because it allows you to adjust the automatic update time from the default. 

You can manually set the delay by including the 'delay' flag which is, unsurprisingly: `-d` (OR `--delay`), when you start htop in the terminal. 

NOTE: The time is measured in milliseconds

So if you want to increase the update time to, oh I dunno, lets say 10 seconds (so that you can see what the hell is actually going on) you would open up a terminal and type this:

    $ htop -d 100

OR this:

    $ htop --delay 100

Now the screen will refresh every ten seconds, instead of the default 'way-too-fast'.

### Username ###

You can easily sort the process list by username (or the process owner). However, you can also start htop with the `-u` (OR `--user`) flag to display only processes owned by a given user.

    $ htop -u username

OR this:

    $ htop --user username

OR this:

    $ htop --user=username

As with other terminal applications, multiple flags can be used to invoke the application, for example you can start htop like so:

    $ htop -d 100 -u username

This will show only the processes owned by `username`, and will refresh the screen every ten seconds.

---

Settings
--------

Htop has a settings menu which allows you to further customise your htop setup. Many of the settings are persistent, which means options you set will remain in place after you close htop, and re open it at a later date.

You can access the settings menu by either pressing <kbd>S</kbd> (uppercase), or pressing <kbd>F2</kbd>.

NOTE: lowercase `s` does not open the settings menu, it does something different. So don't freak out if you accidently type this instead; simply press <kbd>Esc</kbd> to return to the process list.

You will be greeted with a small drop down, cascading menu screen, in place of the process list.

On the far left is the highest level menu titled 'Setup'. It should contain at least these options: Meters, Display options, Colors, Columns.

These are all useful.

You can navigate these menus with your arrow keys in the way that you might expect. <kbd>Up</kbd> and <kbd>Down</kbd> will scroll up and down through a menu, and <kbd>Left</kbd> and <kbd>Right</kbd> will traverse the menu heirarchy.

- `Meters`          -- This menu allows you to customise the layout, and available meters in the top section of htop
- `Display options` -- This option allows you toggle various miscellanous display options
- `Colours`         -- **FUN!** This option allows you to set the colour theme of your htop display (my favourite is 'Black night'!)
- `Columns`         -- This option allows you to toggle the Columns which are on display in the process list, and the order in which they appear

These menus are *fairly* intuitive to navigate, and there's little danger in playing around, so just tinker around and you'll figure it out in no time.

---

Another useful feature of htop, is the ability to kill active processes. There are several ways to achieve this though, which I cover in a dedicated housekeeping [tutorial](memory_management.md)

*And that pretty much covers the majority of the most useful methods for making your way around htop, and finding information you might be interested in.*

---

Tip
---

If you haven't done so already, a really great use case for htop, is taking a look at the process list and checking to see if you have any jobs which are very old. Maybe they're remnants of a job you didn't close down properly, or a system crash, and are now just sitting in suspended animation taking up memory.

You can easily do this by starting htop on the terminal:

    $ htop -d 100 -u your_username -s TIME

Alternatively, you can just start with your username:

    $ htop -d 100 -u your_username

And then click on the `TIME+` Column header, to sort by TIME.

This will list your active processes in descending order of the time since their invokation. If you have any processes older than a day, and you don't have any large data analysis jobs running, which you know to be more than a day old, then this is likely a ghost process, and it can be deleted/killed. This is especially useful if/when these ghost processes are using a particularly large amount of memory (`RES`).

You can also quickly monitor which of your processes are using the most memory by sorting on the `RES` column.

A good habit to get into is quickly checking up on this at least once each day, possibly at the end of the day, as a way to ensure you're not unecessarily using up large chunks of memory when you're not actually working.

Further Reading
---------------

If you want to dig deeper into htop, be sure to check out the man page, which has everything you've read here, and much more:

    $ man htop
    $ htop -h

---

End.

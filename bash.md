Bash and the Terminal
=====================

---

    Author: Geoff
    Last Update: (13-03-2025)

---

Preamble
--------

This is a quick tutorial/reference for getting familiar with the terminal, and the Bourne-Again (bash) 'shell'.

NOTE: I often provide examples of code to type. Whenever you see the `$` dollarsign, this indicates that the code is to be typed on the command line. The `$` is itself representative of the shell prompt. So you wouldn't type it in, but you would type what follows exactly as indicated in the example.

This tutorial is somewhat of a pre-requisite for several other tutorials in this series. Any of them can generally be followed to the letter, without problem. However if you read through this first and apply the learnings until you're comfortable, then you will tackle the other tutorials with *a lot* more confidence!

---

TL;DR
-----

*Bash* is a *shell* program, which runs inside a *terminal* window, and you interact with it on the *command-line*.

Mastering the following ten commands will get you to quickly feeling very comfortable inside the terminal, and working as (or more) efficiently as you would in the standard file explorer in a very short period of time:

1. `ls`  ---  list the contents of the current directory (`ls directory` list the contents of the directory named `directory`)
2. `cd`  ---  change directories (defaults to your home directory `/home/<username>/`, `cd /path/to/directory/` -- change working directory to `directory`)
3. `mv foo bar`  ---  move/rename file from `foo` to `bar`
4. `cp foo bar`  ---  create copy of file `foo` into `bar`:
5. `cat file`  ---  print content of `file` to the terminal screen
6. `head file`  ---  print a 'head' (the first 10 lines by default) of `file` to the terminal screen
7. `grep string file`  ---  search linewise through the contents of `file`, and print all lines with a string matching/containing `string`
8. `touch file`  ---  create an empty file named `file`
9. `rm file`  ---  ***permanently*** remove `file` (some machines will default to asking for confirmation before completing the command)
10. `man exe`  ---  check out the 'man page' (user guide/documentation) for the program `exe` (every one of the above commands has a 'man' page)

A few other quick tips that will make your experience a lot more pleasant:

- tab completion  ---  when typing out long filenames/paths and other executable programs, pressing tab will 'auto-complete' the text for you, if not it's because your current string has multiple matches (press TAB twice quickly to list all matches)
- `*` glob (wildcard)  ---  matches anything (eg. `*.txt` matches all txt files in the current directory
- `~`  ---  this is shorthand for your home directory (`ls ~` will list the contents of your home directory)
- `$HOME`  ---  this is an environment variable that evaluates to your home directory (`ls $HOME` will list the contents of your home directory)
- spaces  ---  if you can at all help it, ***NEVER*** use spaces or special symbols (except `_` or `-`) when creating filenames or directories, you'll figure out why soon enough.
- `/`  ---  The UNIX filesystem can be best thought of as having a heirarchical tree like structure which starts with the (literally) 'root' directory, denoted by a single `/` where every file and directory in a UNIX system can be found in a subdirectory which can be traced back to the 'root' directory.
- use the <kbd>up</kbd> and <kbd>down</kbd> arrow keys to quickly traverse your command history (when working interactively you repeat commands very often!)

Everything you see above is expanded on in more detail below, but if you're keen you can start with the above and figure out the finer details on your own in a fairly short amount of time.

Terminal/Shell/Bash
-------------------

### The Terminal ###

The terminal is a 'terminal emulator' which - for all intents and purposes - just means it is a program that allows you to interact with a shell program, via a command-line. If you want
a history lesson, go check out Wikipedia.

### The Shell ###

The 'shell', is a programming language and also a command interpreter all wrapped up in a scripting environment. It is a program you interact with when using the terminal that you provide commands to.

These commands can be the actual programming language syntax, or they can be you invoking another program (which the shell will execute for you), or some combination of these things.

The shell can be used both interactively, and non-interactively. That is, you can interact directly using the terminal, or you can run a 'shell script' which is a file containing pre written shell commands.

The shell is a somewhat nebulous concept when you're a beginner, and it's relationship to the 'command-line' and 'terminal' can seem very obscure, confusing and mysterious at first. There is really no way around this in my experience. You just kind of use the terminal, and learn about this stuff, and eventually it just sort of makes sense in a vague way after a while.

Worse still, these terms sorta-kinda get used somewhat interchageably. Most commonly is something like 'run X in your terminal', which more specifically means "in your terminal, type X on the command line, and it will be read, interpreted and executed by the shell".

All I'm saying is don't stress. Just roll with it and you'll be fine.

### The Bourne-Again Shell ###

There are many 'shells'. There is the 'C' shell (csh), the 'Korn' shell (ksh), the 'Z' shell (zsh), the original 'Bourne' shell (sh), and more ...

Then there is the 'Bourne-Again shell' (bash or sh) which is a highly developed and improved upgrade from the original Bourne shell.

The Bourne-Again shell (much more commonly known as `bash`) in fact is the default shell on the exceedingly vast majority of UNIX based operating systems today (except for Apple's iOS, which switched to the Z shell a few years ago, which annoyed the hell out of me, but whatever, and there's openBSD, a very UNIX based OS which by default uses a variant of the Korn shell).

All shells have their pros and cons, but `bash` (GNU bash, version 5.2.15(1), is my current version at the time of writing) is the shell we will be using in this tutorial, and I personally don't recommend trying to learn how to use any other shells. Bash is great. It is everywhere, and a significant collection of information online about using a 'shell' generally assumes that the 'shell' is bash.

Most of the information covered in this introductory tutorial will work in most other UNIX based shells, perhaps with some slight variation (hell even Powershell has aliases for many of
these basic commands!)

Besides, when you eventually have the skills and confidence to state with some level of authority that bash isn't what you're looking for, you won't be listening to me anymore.

---

Getting started
---------------

### Getting In ###

Effectively every modern UNIX based OS, also Windows and most other OS's have some form of built-in terminal emulator. It's just a matter of finding it and opening up the program.

In general you'll be greeted with some kind of terminal screen which should look something like this:

    GNU bash, version 5.2.15(1)-release (x86_64-apple-darwin22.1.0)
    Copyright (C) 2022 Free Software Foundation, Inc.
    License GPLv3+: GNU GPL version 3 or later <http://gnu.org/licenses/gpl.html>

    This is free software; you are free to change and redistribute it.
    There is NO WARRANTY, to the extent permitted by law.

    $USER@$HOST: ~ $

Your 'prompt' might look different.

NOTE: As stated, if you're using a Mac, and this is your first time opening the terminal, it will be running zsh by default. You'll know the difference, if you see a `%` instead of a
`$`. You can absolutely learn to use zsh if you like, and chances are everything in this tutorial will work fine, but I can't make any promises. If you want to learn using bash instead, you simply need to invoke it by typing in:

    $ bash

and pressing enter.

Most terminals allow you to play around with the settings to edit a variety of features, such as the window size, the colourization of the display, the font, font size, etc. You can adjust all this to your liking.

Feel free to have fun with that for a while before you move on. The nicer it looks and feels for you personally, the more comfortable you'll be using it, and the more likely you will be to make a habit of using your terminal.

### Interface ###

When you first open your terminal (or start bash), generally you'll be in your home directory. This could be: `/home/<yourusername>/`, `/Users/<ourusername>/` or some variation on that. This will usually be denoted by a `~` before the command prompt.

You'll also see the command prompt `$` and possibly some other information, such as your username and maybe the hostname of your machine also. This 'prompt' can all be customised, but that is beyond the scope of this tutorial (HINT: google 'change PS1 in bash' to get started).

The prompt is, for all intents and purposes, the 'command-line' - it's the line where you enter commands interactively, which are read and executed by the shell.

If you want to see (or confirm) what shell you're currently using type this in:

    $ echo $SHELL

And press enter, and it will print out the file path to the default shell (Spoilers: `$SHELL` is an environment variable!).

You should see something like:

    $ /bin/bash

Or something ending in `bash`. If not then you're not using bash, which is kind of awkward...

### Commands ###

Commands are actually other programs on your machine. they are either 'built-in' programs, or they are external programs. As a beginner, it makes no difference whether they are built-in
or not. The point is that the shell will execute the commands/programs that you tell it to.

Essentially all basic shell commands follow this syntax:

    $ command --flag/s target

That is, you invoke the command of interest, then after some whitespace input one or more modifier flags, then after another space input the target of your operation, most of the time a file or directory. Depending on the program you're executing these flags/targets may be optional. This is all intuitive enough once you start getting used to each command.

Flags come in two basic forms, long and short.

For example:

    $ ls -a
    $ ls --all

are equivalent. The shorthand generally makes it quicker and easier for command line operations, but the long form is (obviously) clearer. Note that the flags are inherent to a given program so they can (and very often do) differ in their meaning and behaviour between programs. 

About the only two flags which *almost* always perform the same function are `-v` (`--version`) and `-h` (`--help`), which most often will print version information and a brief help message, respectively, for a given program. Of course, at least two exceptions to this rule are (incredibly) the very first two examples we encounter below, `cd` and `ls`.

The type of target and the expected behaviour (obviously) differs across programs, so I will just say here that with a basic understanding of a given program, the characteristics and variety of target operations is fairly intuitive, once you get used to them. All shell programs have their own 'man' page which all exist in your system, and their usage is generally only a google away, as we'll see with the simple examples that follow.

Anyway. That's way more than enough boring background stuff.

Let's actually do something useful.

### Basic Navigation ###

One of the most basic and useful things you can do with the terminal, is navigating your filesystem.

When I very first learned how to use the 'terminal', I did it by forcing myself to get into the terminal, and use it everyday to navigate and move and make files, instead of my file explorer. I can't recommend this enough. This helped me to learn and get comfy with basic use in a very short amount of time.

If you want to learn it, you have to use it... every day. The terminal is the steering wheel, brake, clutch and gear stick of your UNIX/Linux operating system. It can do *anything* your file explorer can do, and after a while you'll find it can do many things better and faster.

#### The UNIX Filesystem ####

The UNIX filesystem can be best thought of as having a heirarchical tree like structure which starts with the (literally) 'root' directory, denoted by a single `/`. Every file and directory in a UNIX system can be found in a subdirectory which can be traced back to the 'root' directory.

An important corollary of this is that any file or directory can be referred to explicitly by starting the filepath at the root of the tree like so:

    /path/to/file

Fortunately you don't have to explicitly spell out the full filepath of target directories and files all the time. There are several very useful conventions which make it quicker and easier to define files and directories, relative to where you are.

Firstly, bash will typically interpret any 'naked' target file or directory name relative to the current directory.

Secondly there are the shorthand entries `./` for the 'current' and `../` for the 'previous' or 'parent' directories.

Thirdly bash uses 'tab-completion' to make life much easier. By typing out a bash command, or program, or a file path, you can type the first few letters/symbols and then press <kbd>TAB</kbd> and it will auto-complete the rest of the string for you. If this doesn't work it could be for one of three reasons:

- there are multiple objects bash knows about that match your string, so typing a little more will fix this
- you've made a typo
- the object you're searching for doesn't exist, or bash doesn't know about it

But in general tab-completion makes very light work of searching for files and navigating your file system.

Obviously the two things you'll do the most often in a file explorer, are searching through folders and examining lists of their containing files, which brings us to our first two, and arguably most commonly used commands: `cd` and `ls`.

#### `cd` ####  

Arguably the simplest command you're ever to come across, cd takes a single argument -- the directory you want to get into.

Usage:

    $ cd /path/to/directory

There are only a couple of flags that this command uses, and you are unlikely to ever need them.

More importantly, is a couple of worked examples to illustrate the confusing conventions in the previous section:

    $ cd ../

Will move you from the current directory into its parent directory. So for example from your `Documents` directory pressing `cd ../` will move you back to your home directory.

From my current directory:

    $ cd foo

bash will look only in the current directory for a directory named `foo`, and will move into it. If there is none it will throw an error.

Another very useful command which I didn't get into the habit of using for a long time is:

    $ cd -

This will change you back to *the most recent working directory* (not the parent directory of the current working direcotry). So if you are in your `Documents` directory (`/home/<username>/Documents/`), and change into something else entirely like:

    $ cd /usr/bin/

Then type in:

    $ cd -

It will take you back to your `Documents` (you will then be stuck in an infinte regress, as `cd -` will then take you back again to `/usr/bin/`).

#### `ls` ####  

List directory contents. At its simplest, the `ls` command simply prints all non-hidden files and directories in the current directory across the screen.

However, this command is a real powerhouse. You can print as one line per file:

    $ ls -1

You can include and customise the output of important details:

    $ ls -lA

Include hidden (dot) files:

    $ ls -a

You can sort by filetype, date modified, size, list directories first, and more.

*NOTE: Not all of this functionality is available in every implementation of `ls`. You'll have to read the man pages for your specific implementation to know specifically what you can do with it*

You can list contents of any directory on your machine, that you have permission to read from, even if it's not your working directory, by explicitly specifying the full file path of the directory of interest:

    $ ls -1 /path/to/directory

Will list the contents of `directory`, no matter where you are in the file system.

One of the best things about `ls` is the man page is very reader friendly, so go and check out what you can do with this command:

    $ man ls

And that is about 90% of what you will find most useful when trying to simply navigate your filesystem using the terminal. At this point you can access and view any file or directory, anywhere on your system that you have permission to access.

---

### Working With Files ###

The next most obvious things you will want to do in the terminal is take actions on the files and directories in your filesystem. If nothing else, you'll probably want to create, copy, move and delete files.

This is where, with practise, you'll start to find that the shell is often more powerful, efficient, and precise than using your file explorer for a lot of routine file manipulation tasks.

Especially if you're a professional in tech, and the great majority of files you want to work with are plain text files (.txt, .csv, etc.) and source code (.py, .R, .sh, etc.). 

A dazzling array of programs used on the command line exist specifically for the purposes of manipulating plain text files, including several command-line based text editors (vim, emacs, nano, etc.).

Most of this is well beyond the scope of this tutorial, just know that bash is very powerful.

We're going to cover the following commands, which are all related to creating and moving files and directories around your filesystem:

- `mv`  ---  move
- `cp`  ---  copy
- `rm`  ---  remove (permanently!!)
- `mkdir`  ---  create directory
- `touch`  ---  create file

Then we're going to go through these commands, which are all related to working with the file's contents:

- `cat`  ---  print (conCATenate) files
- `head`  ---  preview the beginning of a file
- `tail`  ---  preview the end of a file
- `grep`  ---  search and print patterns in a file

From now on, most of the commands we're going to use can make real changes to files, and so we need to keep that in mind.

#### `mv` ####  

This command 'moves' files and directories from one location to another. By extension, this command is also used to rename files and directories.

    $ mv foo bar

will rename the file 'foo' as 'bar'.

    $ mv /path/to/foo /new/path/

will *move* the file foo, from `/path/to/`, into `/new/path/`.

You can also move and rename files at the same time like so:

    $ mv /path/to/foo /new/path/bar

`mv` can also be used to move/rename directories:

    $ mv /path/to/directory/ /new/path/

will move the directory named 'directory' (*and all of its contents*) from `/path/to/` into `/new/path/`, so the new file path will be:

    /new/path/directory/

which makes no grammatical sense, but bash doesn't really care about that.

With a little bit of regex, `mv` can be an extremely powerful and efficient command for moving/renaming files and directories en-masse.

Because 'renaming' or 'moving' a file, is really akin to changing the value which points to that file in memory, moving files and directories around inside a single system is extremely efficient.

`mv` also has some useful flags:

- `-f --force` - throw caution to the wind and aggressivley move this file by any means necessary
- `-n --no-clobber` - *do not* throw caution to the wind - do not overwrite anything if the new name/file/location already exists!
- `-u --update` - only move/overwrite if the destination file is older than the source

The next command, for all intents and purposes, works almost identically to `mv`.

#### `cp` ####  

Copy works almost identically to `mv` in most respects, including the behaviour of the above flags.

One instance where it does differ, is by default, copy will *not* copy directories. If you try to copy/rename a directory `cp` will just obnoxiously tell you that it didn't just do what you said to do:

    cp: omitting directory 'foo'

If you want to copy a directory, you must use the 'recursive' `-r` OR `-R` OR `--recursive` flag:

    $ cp -r foo bar

'recursive' means, do this to the directory, and all of its sub-directories, because you're effectively copying all the directories contents also. Unlike `mv` copying a directory and its contents,
involves recreating all that data in a separate location in memory, which can be a lot more time consuming.

Another useful flag that is unique to `cp` is `-p --preserve`. This will ensure that when copying the files/directories they will retain all of their meta-data (author, date modified, file ownership, etc.). 

By default, when you copy a file, the copied version will be a new file, owned by the user who copied it.

#### `rm` ####  

Another very simple command. `rm` *removes* the target from the file system... I.e. it deletes whatever file or directory is the target.

    $ rm /path/to/foo

will remove/delete foo.

The most important thing you need to know about `rm` is that it will *permanently* delete the file or directory from your filesystem. 

> There's no 'trash' in bash.

There is one convenient saving grace though. Like `cp`, `rm` will not remove *directories* by default. To remove directories, again, you must use the 'recursive' `-r --recursive` flag.

NOTE: Even if you use the `-f --force` flag, it won't delete a directory. 

BTW This is not really a 'security' or 'safety' feature. Like `cp` it is to ensure you tell the machine you want to remove the directory
*including all its contents*. It's not necessarily trying to save you from yourself, but it does help.

It might seem scary to think that you can just permanently delete files/directories from your system with a single command. Fortunately on many UNIX machines, there are a lot of other safeguards in place, such as file permissions on directories and files, to ensure that you cannot (without a reasonable amount of effort) do anything truly catastrophic to your file system.

Whilst you can remove/delete any files that you have read/write permission to access, including files and data created by other users, that have full permissions applied to them, 
you can not delete files or directories that you do not have read/write permission to, for example, which is the case for the root `/` directory, and essentially all of the key 
subdirectories containing crucial programs and files.

So don't be afraid. If you're careful about what you're doing, and use common sense, `rm` can be a really useful and lightweight option for keeping your file system clean. Another simple safeguard, is to always try to run a test command first, for example:

    $ ls /path/to/file
    $ rm /path/to/file

Ensure that the output of the `ls` command is exactly (and only) what you want to remove.

PRO TIP: for even more security, after the `ls` command, don't retype the `rm` command from scratch. Instead use the <kbd>UP</kbd> arrow on your keyboard, to go back to the previous command (the `ls` command), and then move the cursor back to the start of the command, delete the `ls` and put `rm` in its place. This will guarantee that you only remove exactly what you just listed!

#### `mkdir` ####  

Real simple. `mkdir` is shorthand for 'make directory'.

    $ mkdir foo

Will create a new, empty directory named 'foo' in your current directory. The only flag which you may find useful is `-p --parents`.

Using this flag means that you will also create any necessary parent directories, if they don't yet exit.

#### `touch` ####  

Maybe the most unobvious and unituitive command in bash. `touch` will, for all intents and purposes, create a new file if it doesn't already exist.

Which you might never guess judging by the name of the command! In any case, it is also about that simple.

    $ touch foo

Technically this command will 'update the timestamp of the target file' 'foo', but if the file doesn't exist, it's created empty, which is what you will use the command for in 99.9999999% of cases.

So there you go...

---

**If you're here you've reached the end of the tutorial, in it's current state. See below for a bit of a peek into the planned topics to be added this tutorial as soon as I can be bothered to
write them.**

- `man`  ---  user MANual for a program
  - built-ins
  - other programs

Regex

Config -- customising (more super powers of convenience)

Variables

Redirection

    >
    >>
    <

Pipes

    |

Further Reading
  - Man pages
  - DataCamp
    - Terminal
    - Shell scripting
  - Grymoire
  - The Bash Guide
  - GNU bash reference/user manual

Glossary of Terms
-----------------

### The Terminal ###

The terminal (or terminal *emulator*) is the program which provides the interface between you and the shell program. Think of the terminal as the viewing window which greets you when you want to work on the 'command-line'.

### The Command line ###

The term used to describe the command prompt that you are greeted with when you open the terminal. I suppose you could just think of it plainly as the line you literally see when you're typing into the terminal.

### The Shell ###

The 'shell', is the command interpreter that you use inside the terminal. When you type in a command at the prompt, it is interpreted and executed by the shell.

### Bourne-Again shell ###

The original bourne shell takes its name from its creator Stephen Bourne, and was the default shell program on UNIX version 7 until it was replaced by the Bourne-Again shell.

The Bourne-Again shell was written by Brian Fox, and developed specifically as a free open-source (GNU) alternative, and successor to the Bourne shell, and released in 1989.

### Built-ins ###

One of the first things we will cover is executing programs in your terminal. It will be helpful early on to understand that there are two types of programs that you can access on the command-line: built-ins, and everything else.

Shell built-ins are programs inherent to the shell, they interact directly with it, and there are important technical reasons for this which you don't need to know about.

### Programs ###

Apart from built-ins, there are all the other executable programs (including ones you write yourself) that can be executed on the command-line (in the terminal, by the shell).

### Environment Variables ###

If you've done any kind of coding, you'll hopefully be familiar with the concept of a variable.

An environment variable is just another variable. Except perhaps only for the fact that it is typically set internally by your environment. Many environment variables contain information set by your machine.

---


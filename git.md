Git and Gitlab
==============

---

    Author: Geoff
    Last Update: (13-03-2025)

---

Preamble
--------

This is a quick(ish) tutorial/reference for making the most of command-line git and gitlab.

NOTE: I often provide examples of code to type. Whenever you see the `$` dollarsign, this indicates that the code is to be typed on the command line. The `$` is itself representative of the shell prompt. So you wouldn't type it in, but you would type what follows exactly as indicated in the example.

This tutorial assumes you are relatively familiar with using a terminal already. At a minimum you should be able to:

- navigate your filesystem (using `cd`)
- display files (using `ls`)
- create files (using `touch`)
- create directories (using `mkdir`)
- remove files (using `rm`)

And probably other stuff I've forgotten. If you're not at all familiar with using a terminal, then I highly recommend checking out my short bash tutorial which you can find [here](bash.md).

I'd like to apologize in advance, this tutorial is a little longer (and wordier) than my others. I wanted to take a little more time with this one because:

- In my humble opinion git is a little more abstract than other tools you use when coding
- A little context and nuance with git ensures its commands are a lot more approachable
- It's not particularly easy to find really good realiable tutorial material for git online (often because they *don't* go into enough detail)
- Even when using stuff like Stack Overflow git, moreso than programming languages like R and python, can still be frustratingly obtuse. Even if a SO post works for you, it can be difficult to know why

So hopefully you'll forgive me for labouring on some of the less 'practical' stuff here and there in this one. Hopefully you'll find that it gives you a little more confidence with git, when all is said and done. 

Or maybe I just like the sound of my own voice...

---

Git
---

So first things first, what is 'git' anyway?

### 'Git' ###

Git is a 'distributed version control system'.

This is an efficient but horribly obtuse way of saying that git is amazing at keeping track of the progress of your code, and it does so in a way that's extremely flexible, by allowing effectively unlimited copies of the entire version history, in multiple locations, in a very memory efficient way.

In short, you can keep track of your changes through time, and have unlimited copies of a coding project on multiple systems, and git makes it really easy to keep track, and communicate across each copy of the code (assuming each copy is accessable across a network... like the internet). 

OK that wasn't much shorter, but hopefully you're getting the idea.

When I say git makes it easy, I define easy rather loosely. It's easy in the sense that git has all the tools you need to do this, and to do it effectively. But whether git is easy to learn and use is a matter of perspective.

Hopefully this guide will help somewhat.

---

Looking at git
--------------

The reason I think git can be quite intimidating to use at first is how seemingly esoteric and mysterious its functionality is[^1].

I mean, at first glance it's not even that easy to 'see' what git is doing at any one time, so you always feel like you're navigating your repository (or repo for short... which is what I'll call it from now on because I'm lazy) with a blindfold on. 

At least it probably feels a bit like those black eye patch things that horses wear to stop them from seeing things beside them. The most obvious example of this is that it can be difficult to even know when git is there watching your code, unless you look for it. The git repo for your code is a little directory in your project folder which is literally 'hidden' from view (`.git/`), like other hidden files/directories on your system it's preceeded by a 'dot'.

Now, without bogging down too much on this, I'd just like to quickly point out that this is also one of git's greatest advantages. Git is intentionally designed to basically never bother you, unless you tell it to.

So what we're going to do is quickly get more comfortable with navigating a repo, and being able to see what's going on, as we work through the first steps of using it.

Specifically, we'll quickly gloss over some of the most basic commands while I show you how to poke around your git repo and get a better feel for what's in it at the same time that we get ourselves set up.

---

### Git Basics ###

As I mentioned earlier, git is a 'distributed version control system'. This is in contrast to a version control system which is not 'distributed', where you typically have a single code base, and users simply read and write directly from/to that codebase.

Git by contrast, allows you to make exact copies of an entire code base, and host them anywhere else, and provides functionality to fairly seamlessly communicate, push/pull code updates back and forth between copies (clones/forks/whatever you want to call them). This has many advantages, most of which I'm too lazy to discuss. But at a minimum, it enables:

- nigh unlimited flexibility
- the ability to code and version control completely offline, if necessary
- the ability to coordinate very large (or extremely small) teams of developers with minimal risk of overlapping development work
- an extremely robust codebase of which there are almost always multiple copies of the entire codebase, stored in a very memory efficient way

In theory you can have as many copies of the code as you want and you can communicate between repositories any which way you like. In practise however, there is most often a single 'primary' remote 'origin' repo, and devs will clone a local copy of that primary one, and push and pull to it alone.

As far as a single repo, or clone of a repo is concerned, it is made up of (surprisingly few) major components.

Wait, what is a repo anyway? I'm sure you're dying to know.

#### Repositories ####

The simplest way to think about a repo, is that it's a directory, with code in it, that git is 'watching over'. It does this by storing snapshots of your 'working directory' (the directory where you're doing your work, that git is watching) into 'commits', whenever you tell it to. 

Then, from here, you can track multiple versions of the code, by creating branches, which allow you to safely track changes to your code separate from other branches by creating commits in parallel. These branches can be merged back together, or branched further. The idea is really simple.

A repo then is not much more than a series of sequentially connected commits, which form a branching pattern from any point where a branch is created, and then merged back into a parallel branch. In fact the way in which commits and branches can be merged can get very fancy, but for the most part you can just think of it as a simple tree diagram of commits, over time.

#### Commits ####

At the heart and soul of git, are commits. A commit is (more or less) a record of the state of your project, at any give time. Commits *are* the version control part of git. They organize and distinguish your code changes, and keep track of one another, and know their place in the history of your code.

Commits are all connected to one another in what can only be described as a 'tree'... which is probably why they're literally called trees.

For the most part, this is the single most important thing you need to know about git. If you can understand that git is really not much more than a collection of interconnected commits which can be created, inspected, navigated, added to, deleted from, tracked, untracked, merged, pushed, pulled, fetched, squashed, rebased, stashed, and much more... then you're already halfway there.

Also, individual files in git are called blobs... sort of. So git tracks individual files by storing them in a blob, which is kind of like a commit, but for a single file.

So to sum up: git is a version control system, which tracks your code in a repo, by creating commits, which are connected in sequence, in a simple tree like arrangement.

So let's make the smallest one we can, then we can actually start looking at it.

#### Installing Git ####

I'm not going to go into any detail here. If you're on any kind of \*NIX machine, odds are git is already installed, or can be done so by sourcing from your package manager.

On windows the process will be somewhat more complex, with entire tutorials specifically dedicated to the task. So I'm going to blithely assume that at this point, you have git
installed and are ready to get started...

#### Creating a repo ####

So if I haven't said it already, the vast majority of this tutorial will be explaining how to use Git on the command line.

A good place to start with Git, is to check that it's working correctly. We can do that by navigating to a terminal, and inputting this command:

    $ git --version

OR this command:

    $ git --help

[Image] -- Stay tuned

The next thing we need to do is tell git who you are. The only reason we're going to do this, is because git won't like it if you don't.

So open up a terminal if you haven't already, and type in these two commands:

    $ git config --global user.name "Your Name"
    $ git config --global user.email Your.Name@ato.gov.au

And don't ask any questions.

OK, now we can make a repo.

So creating a repo (which is short for 'repository' in case you've forgotten already), is as simple as creating a directory somewhere, anywhere, and telling git to watch over it for you.

The canonical name for any directory where we plan on doing any kind of learning is the 'sandbox'. So we're going to create a repo named sandbox.

If you have a particular folder where you like to keep your code/projects, then open up a terminal and navigate to that directory.

    $ cd /path/to/your/favourite/codehub/

Or you can create a directory in your home folder. 

Once you're wherever you want to be, you can create your directory:

    $ mkdir sandbox

Then jump in:

    $ cd sandbox/

Now, there's nothing in here at the moment. You can check by running:

    $ ls -a

[Image] -- Stay tuned

So let's create a repo for a directory which has absolutely nothing in it, just because we can.

Sorry, I should clarify. When I say 'create a repo' I mean specifically 'initialize a repository', which is the more pretentious way of saying it. So we do that by running:

    $ git init

Hence 'initialize'.

Hopefully your terminal spat out a message like this:

    Initialized empty Git repository in /path/to/your/sandbox/.git/

Great!

Now what?

Let's see what happened by typing:

    $ ls

See how there's nothing there. That's what I mean when I say git keeps out of the way.

Now type:

    $ ls -a

If everything went smoothly you should see:

[Image] -- Stay tuned

And there it is, you're git repo! (we're still ignoring those other dots!)

Now that we have a minimal repository, we're going to lean in a bit on how to get in and take a look around. Then once we've covered a bunch of really useful navigation commands, we can
circle back and cover repos, commits and branches in more detail.

The aim here is to make as easy as possible to look around in git, and see what's going on, at any one time as we cover the concepts in more detail.

---

### Basic Navigation ###

OK, let's put some stuff in our repo. We're going to do this part a little quickly, so that we can start taking a look around, and explain a little bit of the process at the same time.

So from within your `sandbox` directory, hopefully you're still there, let's create a small file:

    $ echo "hello world" > newfile.txt

Do another:

    $ ls -a

Now you should be able to see:

[Image] -- Stay tuned

#### Git Status ####

Awesome. We have a git repo, and it has a file in it! Now we can introduce one of the simplest and most important git commands you will ever use:

    $ git status

Type that into your terminal, and you should see something like this:

[Image] -- Stay tuned

Whoa...

Don't panic if you haven't seen any of that before. Git is giving you a *very* brief update on the 'status' of the repo.

If you've done exactly what I've done above, it should be telling you that you're on a branch called 'master', and that you have an untracked file.

And it should tell the name of that file.

So lets start tracking that file:

    $ git add newfile.txt

Now type `git status` again and you see something similar to before:

[Image] -- Stay tuned

Except instead of saying "Untracked files", it should be telling you you have "changes to be committed", and should be recognizing 'newfile.txt' as a 'new file', which I now realise might be confusing... sorry. 

Anyway let's commit that file now:

    $ git commit -m "initial commit"

We've glossed over some more stuff. The `-m` means 'message', and the "initial commit" is the message. This is extremely helpful information, once you know more about what a commit is.

Now if all went well, git will have spat out some more text at you:

[Image] -- Stay tuned

It's telling that you just committed on the 'master' branch, and very briefly summarised the changes made:

  - you changed a file
  - you created a file

It's talking about same file, hopefully that's super obvious.

Now type `git status` again:

[Image] -- Stay tuned

and git should tell you that you're on branch master (it really wants to make sure you always know what branch you're on, because that's really important), and that your 'working directory' is clean. Which I'm sure is a relief.

So, hopefully it's clear by now that `git status` is a really important command that you can use to get a good overview of your repo at any point in time. `git status` will generally give you very helpful information about where you are, what you're looking at, and if there are any changes you need to decide what to do about.

Now that we have made our first commit, we have a commit history.

*NOTE: Very soon we'll go over the process of adding, and committing, and also what branches are in more detail. For now let's just keep 'looking' at stuff, because it's easier to know what you're doing, when you can see what you're doing.*

#### Git Log ####

Now that you have a commit history, you can view your commit history at any time with this command:

    $ git log

This will show you the commit 'hash', and the author of the commit, the date, and the commit message, of all commits in order from most recent, to least, which in our case is only one commit:

[Image] -- Stay tuned

*NOTE: If we didn't earlier tell git our name and email address, then it wouldn't know who the author is supposed to be, and so it would've complained at us when we first tried to do our first commit.*

If the list is longer than your terminal screen it will show you the first page, and you can traverse the list using the arrows keys, <kbd>page up</kbd>/<kbd>page down</kbd>, and <kbd>home</kbd>/<kbd>end</kbd>. If you're really hardcore, you can also use vim keybindings to navigate: <kbd>k</kbd> "up", <kbd>j</kbd> "down", <kbd>ctrl</kbd> + <kbd>f</kbd> "forward one page", <kbd>ctrl</kbd> + <kbd>u</kbd> "up/backward one page", <kbd>gg</kbd> "top of list", <kbd>G</kbd> "end of list"

*NOTE: A commit hash is basically an encrypted form of the changes in a given commit. A commit hash is* ***inherent*** *to the contents of the commit. It cannot be changed or tampered with, either accidentally or maliciously. We're not really going to go in more detail than that in this tutorial. Just know that almost every single commit ever created in the world is unique, and provides a record of git's knowledge of a repo at that moment in time. Therefore commits are a very reliable and totally secure method for version controlling your code.*

Hashes are also why commit "messages" are *super important*. Without them all you have is an unintelligible commit hash, which is great for git, but not great for humans.

Anyway. You can extend the `git log` command to add a dazzling array of customizations to how you view the commit history, which are beyond the scope of this tutorial. But one really nice example is this:

    $ git log --oneline --decorate --graph --all -50 

This is awesome: 

[Image] -- Stay tuned

This will print the 50 most recent commits on a single line each (which for our toy repo there is currently only one), showing only the commit hash and the commit message, and a pretty tree diagram showing how the commits are connected, which if you have multiple branches can be really helpful, and also look pretty cool.

#### Git Branch ####

If you hadn't already noticed, git often likes to remind you what 'branch' we're on. We'll talk more about branches soon, but hopefully what we've touched on so far will give you some intuition about what they are and why they're useful.

This will be a very short section. Type in:

    $ git branch

Git will show you all the branches in your repo, which in our case is only one. It also show's which branch you're currently using with a helpful little asterisk at the beginning.

#### Git Diff ####

So, we can check on the status of our repostory, and we can examine its history. Pretty cool.

The last of the major 'looking around' commands we're going to cover for now is `git diff`, which allows you to examine the changes between almost any two git objects, including between two commits, or two individual files or - most simply - between the changes you've just made and what you've added to the index. You will use this one less often, but it can still be really helpful.

If you've followed everything up to this point, to the letter, you can try running `git diff`, but you'll find it won't do anything. This is because the last thing we did was commit our most recent work. So we have nothing currently 'added' to the index, and we have no 'unstaged' changes (changes not yet 'added' to the index). 

By default, `git diff` will show you the difference between your current changes, and those changes added to the index (which is what happens when you perform a `git add` command). However, you can explicitly tell git what object you want to be able to compare between.

The challenge of git diff, is that you have to be quite specific. For example, if you simply want to compare the difference between two files, on two different branches, you have to be very clear about what files.

For example, take our newfile.txt that we just created, and committed.

At one point in time, this file was 'untracked', then it was added to the index, then it was committed. From git's point of view, these are three *different* files (and they can indeed contain different content, all at the same time!).

So, if you want to compare two files in two different branches, you need to be really specific. We're just going to keep things simple.

From your `sandbox/` directory type in:

    $ echo "how are you?" >> newfile.txt

*NOTE: The two `>>`, instead of one means 'append' if the file exists. So it won't overwrite our previous hard work.*

Now if you type in:

    $ git diff

You should see something like this:

[Image] -- Stay tuned

Git is telling you that there has been a change in the repo. Specifically it's showing you that 'newfile.txt' has been changed, relative to what is stored in the index, and it even shows you what the changes are.

Now if we add those changes to the index:

    $ git add newfile.txt

and run:

    $ git diff

we'll find that again, we get nothing. As I said, by default `diff` only shows you the 'unstaged' changes. We could keep going with this, but honestly, we don't want to get too bogged down. 

We'll finish this section by only adding that if you're interested in viewing the changes between the most recent commit, and the changes in the index you need to make one addition:

    $ git diff --cached

If you run this you'll find the results are the same as the default output, before we added our changes:

[Image] -- Stay tuned

#### Summary ####

So:

  - `git status`
  - `git log`
  - `git branch`
  - `git diff`

are by far the most helpful and common commands that you will use to help navigate your way around your repo, and they can help to provide *a lot* of clarity around your repo, and what your version control looks like at any given time.

A really great habit to get into is everytime you jump into a repo, the first thing you should always do is run `git status` and maybe `git branch` too, to get the clearest update on what's happening at any given time. Git diff can be a little more complicated to use, however a quick google search is usually all you need to quickly figure out how to compare what you're interested in.

If stuff I've mentioned like the 'index', and 'committing' and 'branches' still don't make much sense, don't worry, we'll get to all that soon.

At this point, we should be confident with the basic purpose and usefulness of the above commands, and the value in habitually using them often to keep your head over your repo at any one time.

---

Repositories
------------

We've already talked quite a lot about repositories, and if you've been following along up to this point, you've already created one. What we haven't done is take a more practical look at a git repository, and how to use one in a bit more detail.

So, we've already created a repo, and made at least one commit. If you've followed everything to letter up till now, you should have a git repo in:

    /path/to/your/sandbox/

It should have one file in it named `newfile.txt` (see `git status`).

It should have one commit with the commit message 'initial commit' (see `git log`).

You should have also made one new addition to the file, which has been added to the index, but not committed (see `git status` and `git diff --cached`).

So, what we're going to do now, just for fun, is make another change to our file. So type this into your terminal:

    $ echo "why are you?" >> newfile.txt

At the risk of confusing you, we now have a committed version of `newfile.txt`, we have a version with changes that have been added to the index, and we have yet another version with our new changes, that have not yet been added to the index.

As previously mentioned, we have now 3 separate versions of the file `newfile.txt`, all different from one another. At this point in time, only one of them has been formally and permanently documented by the repo.

If you type in `git status` you should see something like this:

[Image] -- Stay tuned

You have changes ready to be committed, and changes 'not staged for commit'.

Including the committed version, git literally sees three 'different' files.

We could get really fancy here. We could commit the added changes, and leave the untracked changes. They will still be there in the 'working copy', and we could then add and commit those changes also.

We would then have three commits in our history.

Or we could add the new changes now, and they would 'merge' with the changes already sitting in the index. Then when we commit, there would only be two commits.

The key point we're getting at here, is that you have three main 'states' that your files, or changes to them, can be in at any one time.

1. untracked
2. tracked/staged
3. committed

### Untracked ###

There is your 'working copy', which is just the directory where you're doing your work, and all git *ever* really does, is make a note of what's in it.

If you don't make git do anything, it will never do anything to those files except watch them.

### Tracked ###

Then, there is the index. The internet also calls this the 'staging area' (to be fair, git also refers to the contents of the index as changes 'staged for commit'), but it's proper name is the index. This is where git *does* keep track of your changes, but *only* if/what you tell it to track.

### Committed ###

Then, there is your git repo, which is the changes that have been 'committed', formally by git. This repo is the collection of commits, which track the history of changes you have made to the code over time, among other things.

But git will *only* commit changes you tell it to, and only after you have added those changes to the index (i.e. 'staged them for commit').

Sorry if that is starting to sound a bit repetitive, but hopefully that means it's making sense.

This gives you enormous flexiblity though. Because at any time you can stage, or unstage changes at will, and only commit what you actually want to keep, and you have extremely fine grained control over how, and what changes you want to commit.

At least, if you ever want to get that extremely pedantic, you can.

For the most part however, *99% of the time you're going to use these commands*:

    $ git add --all
    $ git commit -m "a succinct and informative commit message"

And you won't really ever deviate from that, unless you want to start getting really fancy.

[//]: # ( ***What now? - try to articulate the general workflow of using git, creating branches, and checking stuff out, merging etc.*** )
[//]: # ( ***What can I do to make this shorter, more succinct, more practical?*** )

Using Git
---------

So, we know that we can create a repo in git, and make changes to our code, and at any time we want we can lock those changes away in a 'commit'.

If you've been following along until now you've already created a repo, added, staged and commited some stuff.

If you're new to git this might still not make much sense. After all, you can commit a file a dozen times, but still only see a single file in your repo. All git is doing in a commit is recording the changes to that file at that point in time. It's not creating a whole 'copy' of the file.

This is where `git log` can be really helpful.

You can look at your commit history, as if it were a list of files, and you see what the messages are, who the author was, when it was created, and you can use the hash to retrieve the
contents of that commit any time you like (We'll cover that a little bit later).

You can retrieve/revert the exact code that was used at any time without having to keep an ever increasing and confusing pile of copies of a file, or no copies at all. Depending on what
you're coding, this has so many advantages, for so many situtations and workflows.

For behemoth projects with hundreds of developers around the world working on major pieces of software with multiple active versions, that all have to be maintianed independently, it's
absolutely essential.

If you work on a data science or analytics team, and you run models on a recurring schedule, and you have to keep track of the code that was used to run a model at a given point in time,
say for auditing or record keeping, this is very easy with git.

If you're a hacker, and you're just working on your own projects, at home, by yourself. Using git can allow you to keep your project tidy and organised, and if nothing else, git commits
can be used to track your progress and make it easy for you to look back over your project and take note of how far you've come. With Github, you also have a permanent backup of your
code, so you can migrate to another machine, or restore your project if your PC is corrupted, or whatever.

You and your colleagues can work on the exact same script, without having to keep copies of your own separate script with your initials at the end. Git organizes and handles all of this for you, and that's really just the bare basics of git.

Now I sound like I'm on a soap box. But the point is, it's easy for users to do this.

Going to (*try*) to move a little more quickly now. Especially as we've already covered most of the following steps already, to some extent.

### Creating a Repo ###

We've already done this, and it's dead easy. Navigate to a directory where you want to host your code (like `cd ~/Projects/new_repo/` for example), and type this:

    $ git init

Your terminal will tell you that you've initialised an empty repository, and tell you exactly where it is.

What's happened is that git has created a repository for you, which is the little `.git/` directory inside your current/working directory. This is where git keeps track of all of the objects and information required to manage your repository.

This is where is keeps records of all of your commits, your tracked files, your repo configuration and more. As a beginner, you don't really need to know what goes on in here, unless you're personally interested. but it's useful to know that all the information you rely on to track your repo history, is in that little folder.

### Committing ###

As previously explained, commits happen in two steps.

    $ git add --all
    $ git commit -m "commit message"

You can add changes from a single file, or multiple files, or all files.

    $ git add filename.txt
    $ git add --all

When you commit, you will only ever commit what has been added to the index. That is, whatever has been 'staged' for commit.

When you commit those changes, you're not making exact copies of those files, only a (cleverly coded) copy of the changes at that point.

One convenient trick however, is that you can 'add' and 'commit' in a single command.

    $ git commit -am "commit message"

The `a` in the `-am` flag is shorthand for 'add'. However, this command will only add *changes* to existing files. It will not add new files to the index. You need to do that the long way.

What happens if you try to commit without a commit message?

Git won't let you (?). What will happen, is that git will prompt you for a message, by automatically opening up a text editor (by default this is 'vi') with a commit message 'file'. As usual it contains a lot of helpful information. It tells you that the first line is the subject line, and you should keep this short and sweet.

After the first line you can include as much detail as you like. Seriously, don't skimp on the details if you don't have to. When in doubt, write more. Once you save and exit the message it will finalise the commit, and include your message.

The default editor is vi. To use vi to edit your script you need to understand how to edit text in vi.

#### The Shortest Vi tutorial ever ####

Vi is a 'modal' editor. That means the keys on your keyboard behave differently in each 'mode'. To edit your commit message you need to enter insert mode, by pressing 'i' (must be lowercase).

Then, you can write text and edit as you normally would. You can delete characters as you would expect, and you can navigate the file using the arrow keys.

Once you've completed your commit message, you need to exit insert mode by pressing your <kbd>esc</kbd> key. Then you need to save and exit by pressing `ZZ`.

If you absolutely can't stand the idea of using vi to edit your commit messages, then you can alter the default editor like so:

    $ git config --global core.editor "nano"

and you're good to go.

Not that I will ever understand why anyone would want to do that...

If you know what's good for you, you might try:

    $ vimtutor

instead, and possibly change your life.

### Branching ###

Hey something new. I don't *think* we've actually created any branches at this point in the tutorial. We've talked about them on and off though. Creating branches is very easy:

    $ git branch new_branch

This will create a new branch named named `new_branch`. 

However, 99% of the time you create a branch, you'll probably want to start using it. So an arguably more useful command is:

    $ git checkout -b new_branch

This will 'checkout' a branch, and if it doesn't exist it will create it. This is convenient, because the command to switch between branches is:

    $ git checkout branch_name

So you may find it easier to remember anyway.

The thing to understand with branches, is they are really just a label for indicating the tail end of a string of commits. In other words, a particular variation of the repo code. 

[Image] -- Stay tuned

If you delete a branch, all you're going to do is delete the label. All the commits for that version of the code will all still exist, so it's a very lightweight and *safe* way to track your changes. 

It also means that after you merge a branch into another, you have two labels pointing to the same commit, which is just silly, so it makes sense to delete one and/or continue to make parallel changes in each branch.

Even better, if you try to delete a branch, when you haven't merged the most recent changes back into a parallel branch, git will complain, and make it difficult for you to do. This is not because you will lose those changes, but because if you delete the branch, you lose a convenient reference to them, which makes it (a little bit) more difficult to get back to them.

It also doesn't make much sense. Why would you create a branch, to work on a specific change to the code, and then not merge that code back into the main version of the code?

Anyway, I digress.

OK, so, when you create a branch it will attach your label to the repo at the point-in-time you created the branch. If you have changes in the index, it will start tracking those changes from the new branch.

So, if you haven't yet, and you're still thinking of this as an interactive tutorial, then lets create a branch:

    $ git checkout -b my_new_branch

Take a look at it with:

    $ git branch

And add some more changes:

    $ echo "where are you?" >> newfile.txt

Hell, let's even create a new file, just to make things more concrete:

    $ echo "this is a new new file" > totallynewfile.txt

Now, when we commit these changes, we will be tracking `my_new_branch` and leaving `master` branch behind.

    $ git add -all
    $ git commit -m "changes to new new file, on my new branch"

Now, when you see the output from your commit, you should see the name of your new branch `my_new_branch` at the start of the message, instead of `master`.

Now we have two branches, and see the history of those branches. My favourite way to do this is with the one liner command:

    $ git log --oneline --decorate --graph --all -50 

---

PRO TIP: Writing this out every time would be horrible. If you type this command in:

    $ alias gitree="git log --oneline --decorate --graph --all -50"

and press enter you will create an 'alias' in your shell's configuration file, which is a shortcut for the command (see the [bash](bash.md) tutorial for more information about aliases). From now on all you need to do is type in:

    $ gitree

To get the same result.

This is one of my favourite git commands. You don't get much better peace of mind that looking at the most recent 50 commits, in a nice tidy tree diagram, with all their messages on
display.

---

In any case, using the above command, you can easily see that `master` is tracking from the previous commit, and `my_new_branch` is tracking from the new commit, and this is the branch your currently working on (as indicated by `HEAD`).

Deleting a branch BTW, is as easy as creating one:

    $ git branch -d banch_name

Remember though, git will make it very difficult to delete an 'unmerged' branch, because git cares. Speaking of merging...

### Merging ###

Merging is about the most anxiety inducing aspect of version control. It's the place where conflicts can occur. Handling merge conflicts is nowhere near as scary as you might think. Git even tells you what to do when it happens:

[Image] -- Stay tuned

A merge is whenever you want to take one commit, and assimilate it into another commit. In general, this is very easy to do in theory. The first thing you need to do is ensure that all relevant changes on both branches are commited. Then (*this is the key*), you need to be 'on' the *destination* branch (that is, you need to `checkout` the target branch, and have its contents in your working directory), the branch you're merging into, not the *target* branch, the branch you're merging 'from'.

So the basic steps are:

    $ git checkout destination_branch
    $ git merge source_branch

The simplest successful merge will give you an output like this:

[Image] -- Stay tuned

In situtations where you have disparate changes between branches, and git cannot as easily merge them together (technically this happens when git cannot 'fastforward' your changes), you will get a message like this:

[Image] -- Stay tuned

In that case, all you need to do is create a 'merge message' and explain what the merge is for. Git will automatically open you're message in your default editor, the same as it will for commit messages (see above)

In situtations where you have mutually exclusive changes across two branches, and git cannot figure out which version/branch is correct, it will throw a merge conflict at you.

#### Merge Conflicts ####

'Conflict' is a scary word. Especially when git spits it out at you in all caps: `CONFLICT`!!

All git is really telling you is that there are multiple versions of a file, and git can't figure out which one is correct. So git just wants *you*, the human with a brain, to take a look and tell git which changes it is supposed to keep.

This is *literally* as easy as opening the file/s where the 'conflict' occured, and deleting the redundant section, and keep the good section. Then as soon as you've saved the final version of the file/s, you simply need to add and commit those changes anew.

That's all there is to it.

So handling a merge conflict is a easy as deleting the unwanted code, in theory. What can make merge conflicts more difficult, is the extent of the changes that you need to interrogate. If two branches have been diverged for a long time, and merged back together without significant forethought, then you can end up with quite a mess of conflicts that all need to be dealt with separately.

But that's not really git's fault.

In the smallest example, git will see where two scripts contain mutually exclusive code, and will give you both versions to inspect yourself in the following way:

    <<<<<<< HEAD
    Changes to HEAD (the current branch)
    =======
    Changes to target branch
    >>>>>>> target_branch

So it's simply a matter of keeping the changes you want, and discarding the rest (including git's helpful pointers):

    Changes to target branch

Or whichever you want. Once you have completed the updates and fixed all your code. You can then add your changes, and commit as you normally would (which you can do the quick way using `-a` - `$ git commit -am "fixed merge conflict"`).

And that's all there is to it, really.

#### Avoiding Merge Conflicts ####

More important than handling merge conflicts, is learning how to avoid them, or minimise the risk of them, or at the very least making them as simple and manageable as possible.

90% of handling merge conflicts is good team communication, and good devOps.

Technically handling a merge conflict is about ensuring that the same code/lines of code, in the same file/script has not been edited separately on two branches (or clones) in parallel, without changes being merged in sequence.

A few simple guidelines will avoid most merge conflicts:

- Always pull changes from the remote repository (see below) before you update and push your local changes to the origin
  - Even if you get a conflict the changes will be local, so you can fix them before then pushing back to the remote
- Get into the habit of pulling at least once per day (in addition to pulling whenever you plan to update the remote) especially at the beginning of your day
- commit and update the remote origin regularly, like at least once per day, but perhaps more often
- communicate regularly as a team and ensure the team is generally aware of who is doing what work
- Never do any development work on the master/main branch - only ever merge from develop, and only code that has been thoroughly tested and validated

Though there is a bit of art to this. You may like as a team to really only commit when you make concrete changes to your code, or at key checkpoints. The main idea though, is to avoid lengthy periods of time between commits. Keep them tight and sensible, if only to ensure that changes in each commit are easily comprehensible and maintainable.

Just remember that when merge conflicts do (inevitably) occur, the more regularly the code is being committed/merged the smaller and easier the conflicts will be to examine.

### Tags ###

Tags are awesome. Tags are a really intuitive, and obvious way of ear-marking specific commits in your history, that have some special value. In software development tags are most often used to mark specific versions/releases/patches of production code.

There are two types of tags, lightweight and annotated. Annotated tags are way more useful, and recommended in the vast majority of situtations, and they're not particularly difficult to create; so I'm only going to go over them here.

To view your existing tags, if you have any, type in:

    $ git tag

To create an annotated tag, is not unlike creating a commit:

    $ git tag -a tag_name -m "an informative and succinct tag message"

And it's that simple. If you run statistical models in regular intervals, a useful tag rubric might be:

    $ git tag -a 2023_03_MX -m "No changes from previous run. No errors. Results can be found here: archive_dir"

This will add the annotated tag to the most recent commit in your repository. This tag will contain the name of the person who created the tag, the timestamp, the tag message and the commit hash, among other things.

You can also tag any commit in your history, even old commits, by simply specifying the commit hash at the end of the tag command:

    $ git tag -a FY2023_Q3_SMB -m "No changes from previous run. No errors. Results can be found here: archive_dir" 3cb526d

This will add the tag to the specified commit.

Now you have a completely secure, tamperproof commit, tagged with useful information about that specific commit.

And that's how you version control you're code.

Pretty cool.

### Ignore files ###

We're almost done with repositories, and commiting, and version controlling. There's one last really useful thing to cover before we move on to working with remotes.

Almost certainly, your working directory will contain certain files/directories and other paraphernalia, that you *don't* want to keep in git's repository. I.e. you don't want git tracking those files for various reasons. So you can ignore them by including them in an ignore file.

Git has multiple ignore files in different places, for different reasons. The most important one is the local ignore file. This is a file that you keep in your working directory named `.gitignore`.

Git knows this file, and will instantly recognise it and know what to do with it. Like most other files in your repo, you will want to version control it, so you'll need to track it like anything else.

Real simple. This file will contain line by line strings of any/all files that you don't want to be tracked by git. for example:

    .Rdata
    .Rapp.history

Are files you almost certainly don't want git tracking.

Slightly less simple. `.gitignore` also accepts REGEX. If you don't know what that is google it. It is essentially a syntax for using special characters to designate groups of other characters. For example `*` is a 'wildcard' character which more or less means, 'zero or more of anything'. So:

    *.txt

evaluates to 'literally any files ending in `.txt`.

All you need to do is create your `.gitignore` file, and enter the string/s of any files that you don't want git to track, one on each line, and then save and commit.

BTW lines beginning with `#` are regarded as comments in `.gitignore`, and are *ignored*, by your `.gitignore`... if you can bend your mind that far.

OK... so you can:

- create repositories
- visualise them in a number of ways (`git status`, `git branch`, `git log` just to name a few)
- commit changes
- create branches
- merge
- handle conflicts (REMEMBER: Just look at the files/conflicts, keep what you need, then add and commit and BAM, you're done!)
- tag
- ignore files

There's not a lot that you can't do now. The only thing left really, is how to manage remote repositories, and keep them up to date.

Sure there's resetting, rebasing, rerere, and other tricky stuff. But that stuff is icky and dangerous; and frankly, pedantic.

There is version control, where you keep track of the history of your code.

Then there is bonafide repo hacking, where you can really go to town, and construct a repository and history almost any way that you like. This just isn't necessary or pratical, unless
you have ambitions to play around with the laws of nature.

Remote Git
----------

### Setting up an Account on GitHub ###

[//]: # ( I need to do this at home -- using the internet )

What do I want here?

- registering is easy, go follow the instructions

Before you can access the GitHub instance on an AQZ machine, you must create an account. To do so, simply open up FireFox, and navigate to the GitHub login page: `http://localhost/users/sign_in`

At the bottom of the page is a link to 'Register now'. All you need to do is follow that link, and provide all the necessary information.

- Name: You should use your preferred full name
- Username: Your atonet username as your 
- Email: Your atonet email `Your.Name@ato.gov.au` - the same one you registered in your global config (see above)
- Password: *you should register with a* ***different*** *password to your atonet password!!* - Your GitHub password will not be required to be changed every 90 days, so will inevititibly become different, and you'll likely forget what it is!

Once you do this it is also helpful to send an email through to the GitHub admin (at the time of writing this is `Geoff.English@ato.gov.au`).

You should recieve an email, when your account has been approved.

Once you're account has been activated, notify your manager and/or the model owner/s of any model/s repositories you require access to, and they will add you to the relevant group.

### Setting up an Existing Repo on GitHub ###

It's very common to have a git repo that already exists, that you want to migrate and host on GitHub.

If you've been following along, then you have a repo that you've created, which you can host on GitHub, once you've registered for an account.

The high level steps for this process are:

- Create a new 'Project' in GitHub using a name, as close as is practicable, to the name of your repo (i.e. the name of the parent directory containing your repo)
  - If you plan do push an existing repo, then ensure that the created project is completely empty (decline to create a README and/or a `.gitignore` file, or anything else)
- Copy the project url (HTTPS)
- Follow the instructions in GitHub to push your existing code from your local repo into GitHub

GitHub helpfully provides instructions for a variety of scenarios. For a repo you've already created GitHub gives you all the terminal commands you require (including the URL):

    cd existing_repo
    git remote rename origin old-origin # This step is only necessary if you already have your local repo setup to track another remote -- if this repo only exists locally, you don't need to rename 'origin'
    git remote add origin <project URL>
    git push -u origin --all
    git push -u origin --tags

You can copy/paste the commands provided in GitHub, except the first one. Obviously you need to `cd` into the specific directory on your machine.

### Cloning a GitHub Repo ###

Cloning a git repo is very easy.

In order to clone a repo you need to know the location of the remote repo. You can get this by navigating to the remote repo in GitHub, on FireFox.

After logging in, you can navigate to the repo you’re interested in, and find the bright blue "clone" box. From the drop down, you can either select/copy the link, or click the clipboard icon to the right.

***IMPORTANT:*** On the AQZ machines, we are **only able to use HTTPS protocol** (not SSH!). So ensure you copy the HTTPS link.

Now open up a terminal if you haven't already, and navigate to your favourite projects directory.

    $ cd /path/to/favourite/projects/folder/

From that directory, you can simply clone the repo directly:

    $ git clone <paste url>

You *don't* need to create the repo directory first. When you run git clone, git will create the repo directory for you, and copy all the code into that directory.

Now you can confirm that the repo has been successfully created when you see something like this:

[Image] -- Stay tuned

You can also confirm this by taking a look at your current directory:

    $ ls

You should be able to see a directory named after your new repo, which you can now navigate into:

    $ cd <repo name>

And finally confirm further that this is your repo by looking in the directory for your `.git/` directory, indicating that this is a git repo:

    $ ls -la

You should also see the contents (if any) of your cloned repo.

You can now checkout all the branches in this repo:

    $ git branch

Or get a status update:

    $ git status

Or look at the commit history:

    $ git log

It's all there, just as if this repo had been on your machine the whole time.

Now you can start creating your own branches, editing code, creating commits etc. All of these changes will only be present in your local clone of the repo, until you push them remotely.

### Pushing to a Remote Repo ###

Once you have cloned a repository, you now have a full copy of that repository, and access to it, and all of it's branches in your local project folder. 

You can monkey around in that repository all day long, and save, add and commit anything, anywhere you like and it will have no effect on the remote repository hosted on GitHub.

---

***The remainder of this tutorial is in active development...***

---

    $ git pull
    $ git push origin <branch name on remote>

Slightly beyong the basics:

    $ git stash
    $ git configs??
    $ rebase/reset/cherry-pick/rerere


### Important Stuff to Keep In Mind when working with Remote repos ###

- *always* pull or fetch/merge from a remote repo *before* you push any changes from your local copy
- ***NEVER (EVER)*** 'amend' a commit which has *already* been pushed to a remote repo
  - There's generally no real need to ever amend a commit. Best just to make changes, however small, and commit them anew and push the changes
  - If you don't know how to 'amend' a commit, don't stress, you're not missing much

---

Footnotes
---------

[^1]:The other thing that makes git a challenge, is that you really don't use it all that much. Nine times out of ten, on a work day, your only interaction with git is:

    `git add .`
    `git commit -m "unhelpful commit message"`
    `git push origin main`

If those three commands work without any issues then you're on cloud nine. Contrast this with spending several hours every day writing code, which absolutely needs to work or you fail at your job. With git, something might go wrong once in a blue moon, and you rarely ever spend so much time fixing stuff that you know from memory, exactly what to do. So it can really take time to master, and usually involves no small amount of doomstacking.

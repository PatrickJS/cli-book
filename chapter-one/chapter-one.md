#Chapter One - Overview of Command Line Tools

Feel free to skip or skim this chapter if you use the command line often enough. I'm going to give a tour of the command line, its syntax and talk a bit about running commands.

##What is it?

The command line is simple. It's a place where you can type in commands for your computer to run. You computer will atempt to run the command, whatever it may be, and report back any results. The command line allows you to tell your computer exactly what you want it to do. You computer will do whatever you tell it, so make sure not to issue commands that will cause harm to your machine.

We are going to be creating a ton of commands thought this book. It's probably a good idea to be familiar with running commands and navigating your file system via the terminal.

Let's start by getting you into the command line, and then we will run some basic commands.

##Accessing the command line

Depending on your operating system, it may be called something slightly different. In OS X it is refered to as the terminal, in Windows it is call the command prompt. I will use terminal thoughout this book, but for no particular reason other than a preference.

**Windows** - If you are running windows, you can open the terminal by pressing ```window + r```. You can then run "cmd" to launch and intance of the command line.

**OS X** - If you are on a Mac you can open the command line by accessing ```Applications/Utilities/Terminal```.

On any system, you should be able to seach for command line or terminal and find the systems terminal.

##Syntax

The black screen in front of you might look a daunting, but it really is simple to break down.

*picture*

Everything before ```:``` is my username. Then we have the current working directory path followed by a ```$```. These are going to be display everytime the terminal is ready for a new command. They let you know you can issue a new command, and the provide some context to who is using the computer and what folder they are in.

Everything after the ```$``` is editable. Here you can type in the command you want to run.

Once you issue a command, the computer may print out some results or errors. These will be shown on lines without this prompt format.

To better understand the syntax, lets take a look at the worlds simplest command, ```ls```.

##Running a command

Remember that current working directory we talked about? While if you issue the `ls` command, it will show you the folders and files in that directory. I'm going to navigate to the folder that contains the contents of this book and run `ls`, the result is below.

	PHLMLAMEAD:~/code/creating-clis-in-nodejs$ ls
	chapter-one	  overview	 readme.md
	
	PHLMLAMEAD:~/code/creating-clis-in-nodejs$ 

Above you see that same prompt we discussed above. It show who is issuing the command, and what folder they are doing it from. As you can see, I'm running the command from the `creating-clis-in-nodejs/` directory. Then I type in the command I want to run, which in this case is `ls`.

The next line is the results of the command. Notice there is no prompt on this line, it starts with the results. In my case, its two folders, `chapter-one` and `overview`, and a file called `readme.md`.

After the results from you command, the terminal is ready for more!

##Arguments and flags

Commands like `ls` can take other inputs as well. These come in the form of arguments and flags.

Flags are are options that can alter the operation of a command. They start with one or two dashes (`-`). They can operate alone or they might take an additional argument with a space inbetween the two. Let's take a look at a flag that the `ls` command supports.

	PHLMLAMEAD:~/code/creating-clis-in-nodejs$ ls -l ~/code
	total 75552
	drwxr-xr-x   5 andrew_mead  1804680031       170 Oct 18 15:59 backbone-playground
	drwxr-xr-x   7 andrew_mead  1804680031       238 Oct 20 18:57 creating-clis-in-nodejs
	drwxr-xr-x  17 andrew_mead  1804680031       578 Oct 21 16:07 hay
	-rw-r--r--   1 andrew_mead  1804680031  38680846 Oct 18 16:39 hay.zip
	drwxr-xr-x  12 andrew_mead  1804680031       408 Oct 20 18:03 node-progress

Notice we are running the same command, from the same folder, yet we are getting a much different result. This is because of the `-l` flag. The `-l` flag lets you provide a custom location to print the contents of. This is provided after a space in `-l`.

Not all flags require arguments, and not all arguments need flags. Let's take a look at the `mkdir` command, to see this syntax.

	PHLMLAMEAD:~/code$ mkdir some-folder
	
	PHLMLAMEAD:~/code$ ls
	backbone-playground	       hay		    node-progress
	creating-clis-in-nodejs	   hay.zip		some-folder
	
	PHLMLAMEAD:~/code$ 


The above syntax runs a command that will create a directory in the current working directory. It also take an arguments which will be the name of the new folder. If you want to have spaces in your folder name, you will need to wrap it in quotes.

If I run `ls` again, you can see that new folder was successfuly created.

##Basic navigation

We will be doing a lot of work in the terminal, and it's important to know how to get around. This is done via the `cd` command which will change directory.

I you want to go into a folder in your current working directory, you can run the `cd` command, where the first arument is the path to go to.

	PHLMLAMEAD:~/code$ cd creating-clis-in-nodejs/
	PHLMLAMEAD:~/code/creating-clis-in-nodejs$ 
	
Notice the change in the path in the prompt. It now shows my updated location. You can also go backwords by running the following.

	PHLMLAMEAD:~/code/creating-clis-in-nodejs$ cd ..
	PHLMLAMEAD:~/code$

If you want to travers outside of your current location, you can run `cd /` to get to the root of your computer, or `cd ~` to get to your user directory.
 
##Where are the commands comming from?

You are running all these built in commands, but where are they coming from? In the case of osx you can find all of these in /bin. If you want to see for yourself run `cd /bin`. Then run `ls` to list out the directories contents.

The system knows to look in this folder for commands. When we create our own commands using Node.js we will need to alert the system of our commands location.
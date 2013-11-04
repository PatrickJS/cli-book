#Chapter Two - Creating your first command line tool

Okay, creating first command line tool is going to be simple. In the chapter we are going to setup a seed project you can use for all your future command line tools, and we'll go over the basics to creating a command line tool that anyone can install via `npm`.

`npm` is the package manager that is bundeled with node. It provides an easy way for people to install software written in Node. NPM is bundled with Node, and will let anyone with Node install your tools in a couple seconds.

##Prerequisites

All you need to do is intall Node. Node comes bundeled with npm, so head over to [Node.org](http://nodejs.org) and run the installer.

To confirm your installtion, pop up a terminal and check check that everythings working.

	PHLMLAMEAD:~$ node -v
	v0.10.13
	
	PHLMLAMEAD:~$ npm -v
	1.3.2
	
	PHLMLAMEAD:~$ _

`-v` flag is a common command to check the version of the software.


##Directory structure

The following is the directory structure for our seed projects. Check it out, then we'll break it down.

	/project
	  package.json
	  /bin
	    hello
	  /lib
	    hello.js

###package.json

`package.json` is the configuration file for your npm package. It's required for any npm module and is placed at in the root of your npm package. This is a configuration file that let's npm install dependencies, lets users find your package on npm.org, and provides a way to initiate commands for running and testing the package.

Some specific fields you can provide are:

* `name`
* `version`
* `dependencies`
* `repository url`

I wouldn't dare try to cover the details of this file here. For that, check at the documenation at [npm.org](https://npmjs.org/doc/files/package.json.html).

###/bin and /bin/hello

The bin directory contains files whos names represent commands that will be available once the module is installed. In this case, `/bin/hello` will register a command `hello` which you could run like this:

	PHLMLAMEAD:~$ hello

You can call the folder anything you like, but the name `bin` provides some context about its contents.

### /lib and /lib/hello.js

The `/lib` directory is going to contain all the code that we need to run the commands. When someone runs the command `hello` from the terminal, the file in our `/bin` directory is going to call something in our `/lib` folder. This is done for clarity and reusablility. If you had a npm modules that has terminal access and could be used in someone elses project, you would not want two files repeating the same code.

##Global v. location installation

I want to touch on this real quick because it's important. There are two ways someone can install your npm modules, locally and globally.

	PHLMLAMEAD:~$ npm install hello
	
If a users installs you module locally, these modules will only be accessable from within that directory, and terminal commands will not be registered.

	PHLMLAMEAD:~$ npm install -g hello

If a user installs your module globaly, it will register potential terminal commands with the terminal.

Our commands will always require global installation so users can access them via the command line.

##Running npm init

Most of your `package.json` file is going to be auto generated. All you need to do is create a directory to house your package, navigate to it in your terminal, and run `npm init`. This will take you though a series of questions about your new package.

The process is shown below.

	PHLMLAMEAD:~/Desktop/hello$ npm init
	This utility will walk you through creating a package.json file.
	It only covers the most common items, and tries to guess sane defaults.
	
	See `npm help json` for definitive documentation on these fields
	and exactly what they do.
	
	Use `npm install <pkg> --save` afterwards to install a package and
	save it as a dependency in the package.json file.
	
	Press ^C at any time to quit.
	name: (hello) 
	version: (0.0.0) 1.0.0
	description: A demo npm package
	entry point: (index.js) ./lib/hello.js
	test command: 
	git repository: 
	keywords: 
	author: Andrew Mead
	license: (BSD) 
	About to write to /Users/andrew_mead/Desktop/hello/package.json:
	
	{
	  "name": "hello",
	  "version": "1.0.0",
	  "description": "A demo npm package",
	  "main": "./lib/hello.js",
	  "scripts": {
	    "test": "echo \"Error: no test specified\" && exit 1"
	  },
	  "author": "Andrew Mead",
	  "license": "BSD"
	}

Note that this creates the `package.json` file for you. This is a barebones configuration. We will need to add a couple attributes to allow it to be installed as a global package that adds terminal commands.

##Configuring package.json for command line tools

There are a couple specal attributes you need to configure to allows a global installation to register commands with the command prompt.

###Prefer Global

The first property to add is ```preferGlobal```. preferGloabl is a handy little property that tells npm this module is intended to be install globally and used from the terminal. If a user tries to install the module locally, npm will warn them that the package is intended for global installation.

todo - add the entire file so they know

```
{
  ...
  "preferGlobal": "true",
  ...
}
```
###Directories Property

The ```directories``` property brings all our files together. It lets us tell npm which directories contain commands we want the user to access from the terminal.

```
{
  ...
  "preferGlobal": "true",
  "directories": {
    "bin": "./bin"
  },
  ...
}
```
In the above snippet we tell npm that we want our bin folder to be registered as the folder that contains our commands. This will let a user type `hello` in the terminal, and get a real result.

###Final configuration

My final `package.json` file now looks like this:

	{
	  "name": "hello",
	  "version": "1.0.0",
	  "description": "A demo npm package",
	  "main": "./lib/hello.js",
	  "scripts": {
	    "test": "echo \"Error: no test specified\" && exit 1"
	  },
	  "preferGlobal": "true",
	  "directories": {
	    "bin": "./bin"
	  },
	  "author": "Andrew Mead",
	  "license": "BSD"
	}

##Writing some code

Enough setup, let's write a little code.

We are going to create a command line tool that will print out a generic gretting to the user. The user can also specify their name

First things first is our `/bin/hello` file. This is the only file that's going to get executed when the command is run from the terminal. Let's check out is contents:

**/bin/hello**
	
	#!/usr/bin/env node
	
	require('./../lib/hello.js');

It's only 2 lines long, and they are both pretty important.

The first line is called a shebang. It's prefaced with a `#!`. This tells the operating system that when this file is run as a program, run it with the following program. The rest of the file is run with the specified program.

In this case, the second line is the line that is run by node.

In line 2 we bootstrap out commands main file, which is located in `/lib/hello.js`. All we do here is execute the library file, so let's create that.

**/lib/hello.js**

	process.stdout.write('Hello World!\r\n');

All this line does is write our message to the terminal, and go to a new line.

##Installing and running your module locally

Let's install our module to test it out. In the terminal, navigate to the root of our package (the same place you ran `npm init` from).

Run the install command, specifying the curreny directory as the location for installation.

	npm install -g ./

`npm intall` lets you install a new package. The `-g` flag tells npm to install the module globally. `./` points to the current directory where npm will look for the `package.json` file that you created.

Once you are returned to a new prompt, you package is installed! You will need to restard your terminal, or open a new tab.

You can now run `hello` from the command line and you should see something like this:

	PHLMLAMEAD:~/code/hello$ hello
	Hello World!
	PHLMLAMEAD:~/code/hello$ _

##Uninstalling

Uninstalling any npm module is easy. To uninstall our hello package, run:

	npm uninstall -g hello

You need to include the `-g` flag if you originally installed the package globally.

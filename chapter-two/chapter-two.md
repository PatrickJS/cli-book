#Chapter Two - Creating your first command line tool

Okay, creating first command line tool is going to be simple. In this chapter we are going to setup a seed project you can use for all your future command line tools. We'll go over the basics to creating a command line tool that anyone can install via `npm`.

`npm` is the package manager that is bundled with node. It provides an easy way for people to install node modules. This will make a great distribution platform for the tools we create.

##Prerequisites

All you need to do is install Node. You can download the installer at [Node.org](http://nodejs.org).

Open a terminal and check that node and npm installed correctly. You can do that by checking the version for each.

	PHLMLAMEAD:~$ node -v
	v0.10.13
	
	PHLMLAMEAD:~$ npm -v
	1.3.2
	
	PHLMLAMEAD:~$ _

`-v` flag is a common flag used to display the version of the software.


##Directory structure

The following is the directory structure for our seed project. Check it out, then we'll break it down.

	/project
	  package.json
	  /bin
	    hello
	  /lib
	    hello.js

###package.json

`package.json` is the configuration file for your npm package. It's required for any npm module and is placed at in the root of your projects directory. `package.json` lets npm install dependencies, lets users find your package on npm.org, and provides a way to setup commands for running and testing the module.

Some specific fields you can provide are:

* `name`
* `version`
* `dependencies`
* `repository url`

I wouldn't dare try to cover the details of this file here. Visit [npm.org](https://npmjs.org/doc/files/package.json.html) for a complete description.

###/bin and /bin/hello

The bin directory contains files whose names represent commands that will be available once the module is installed. In this case, `/bin/hello` will register a command `hello` which you could run like this:

	PHLMLAMEAD:~$ hello

You can call the folder anything you like, but the name `bin` provides some context about its contents.

### /lib and /lib/hello.js

The `/lib` directory is going to contain all the code that we need to run the commands. When someone runs the command `hello` from the terminal, the file in our `/bin` directory is going to call something in our `/lib` folder.

We could easily store everything in `/bin/hello`, but doing so would create code that is not reusable. It would be better to create a reusable API in `/lib/hello.js` that can be used globally via our command line tool, and reused locally within a separate node application.

##Global v. location installation

I want to touch on this real quick because it's important. There are two ways someone can install your npm modules, locally and globally.

	PHLMLAMEAD:~$ npm install hello
	
If a users installs you module locally, these modules will only be accessible from within that directory, and terminal commands will not be registered.

	PHLMLAMEAD:~$ npm install -g hello

If a user installs your module globally, it will register potential terminal commands with the terminal.

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

To allow your script to be run from the terminal, there are a couple of configurations in `package.json` you need to be aware of.

###Prefer Global

The first property to add is `preferGlobal`. `preferGloabl` is a handy little property that tells npm this module is intended to be install globally and used from the terminal. If a user tries to install the module locally, npm will warn them that the package is intended for global installation.

```
{
  ...
  "preferGlobal": "true",
  ...
}
```
###Directories Property

The `directories` property brings all our files together. It lets us tell npm which directories contain commands we want the user to access from the terminal.

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

It's time to add some functionality to the module. The module going to provide a generic greeting to the user and allow the user to specify their name for a personal touch. The user will be able to specify their name via an argument.

The first file to edit is `/bin/hello`, which is the entrance point of the script. Below is the final version.

**/bin/hello**
	
	#!/usr/bin/env node
	
	require('./../lib/hello.js');

 This file has three jobs:

### 1. Provide a name to register with terminal

What are uses going to type in the terminal to run the command? This file name is "hello", so uses will run the command by issuing the following:

	PHLMLAMEAD:~/code/hello$ hello

### 2. Specify an environment to run in

If the file contains javascript code, which this file does, it needs to instruct the computer to run this file with node. This is what the first line of the file does, and it is called the "shebang".

	#!/usr/bin/env node

Prefaced with "#!", the shebang provide the program that the rest of the files contents should be passed to. In this case, the rest of the file is passed into node, and is run as javascript.

### 3. Bootstrap functionality

Fo the sake of simplicity, `/bin/hello` intentionally contains very little. The second line is in charge of starting the library script. It uses a `require` statement and passes in the path to the library.

	require('./../lib/hello.js');

-----

The only other file is `/lib/hello.js` which contains the code to execute. 

**/lib/hello.js**

	process.stdout.write('Hello World!\r\n');

All this line does is print out "Hello World!" and the move to the next line.

##Installing and running your module locally

To install the module, navigate to the projects root directory in the terminal and run:

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

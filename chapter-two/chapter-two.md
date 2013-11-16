# Chapter Two - Creating your first command line tool

This chapter is going to provide you with the knowledge to create your first command line tool. We'll setup our dependencies, configure our first module, and create our first tool.

## Dependencies

To get started make sure Node.js and npm are installed on your system. You can download the latest version of node at [nodejs.org](http://nodejs.org). npm is bundled with Node.js. There is not need to install it separately.

npm is the package manager that is bundled with node. It let's you install modules that others have created, and publish your own modules. You can check it out at [npmjs.org](http://www.npmjs.org).

To check that the installer ran correctly, run the following commands in your terminal

	PHLMLAMEAD:~$ node -v
	v0.10.13
	
	PHLMLAMEAD:~$ npm -v
	1.3.2
	
	PHLMLAMEAD:~$ _

You should see similar output. The `-v` flag is a common flag used to display the version of the software you have installed. It also implicitly let's you know if the software is installed.

## Modules Directory Structure

Next up is creating the directory structure for our module. I would recommend something like this:

	project/
	  package.json
	  bin/
	    hello
	  lib/
	    hello.js


### project/package.json

`package.json` is the configuration file for your npm module. It's required for every npm module and is placed at in the root directory. It provides valuable information regarding module dependencies, general module information, and other configuration options.

`package.json` is where you would specify the module name, version number, repository url, license, and author.

For a complete listing of configuration options, checkout the documentation at [npm.org](https://npmjs.org/doc/files/package.json.html).

### project/bin/

The bin directory contains commands we want to our module to add to the system PATH. The files in bin will need to be configured in `package.json` which is done later in this chapter.

The name "bin" is descriptive but not required. It can be called anything you like.

### project/bin/hello

`hello` is going to contain the necessary code to run the command line tool. Because it is in the bin directory, it will register the command `hello` to be accessible via the terminal.

### project/lib/

This is the library folder which will contain the majority of the source code. While `hello` in `project/bin/` is the entrance point for our tool, the bulk of our code will reside in `project/lib/`.

### /project/lib/hello.js

This file will contain the functionality for the `hello` command.

All the javascript could be stored in `project/bin/hello`, but that's poor practice. It's cleaner and more reusable to have `hello` bootstrap code inside `hello.js`.

## Running npm init

To create a new npm module, run `npm init` from the terminal. This will provide a wizard for setting up a basic module configuration.

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

Note that this creates the `package.json` file for you. This basic configuration is a great starting point, but requires a few tweaks to suit our needs.

## Configuring package.json

To allow your script to be run from the terminal, there are a couple of configurations in `package.json` you need to be aware of.

### Prefer Global Installation

`preferGloabl` is a handy little property that tells npm this module is intended to be install globally and used from the terminal. If a user tries to install the module locally, npm will warn them that the package is intended for global installation.

```
{
  ...
  "preferGlobal": "true",
  ...
}
```
### Specify PATH Files

To access files from the terminal, they need to be added to the PATH. npm let's you specify this via the bin property. This property takes a hash of commands and files to run.

```
{
  ...
  "preferGlobal": "true",
  "bin": {
        "hello": "./bin/hello"
  },
  ...
}
```

The above will register hello to the PATH, and it will execute our `project/bin/hello` file.

### Final configuration

The final configuration should look similar to this.

	{
	  "name": "hello",
	  "version": "1.0.0",
	  "description": "A demo npm package",
	  "main": "./lib/hello.js",
	  "scripts": {
	    "test": "echo \"Error: no test specified\" && exit 1"
	  },
	  "preferGlobal": "true",
	  "bin": {
        "hello": "./bin/hello"
      },
	  "author": "Andrew Mead",
	  "license": "BSD"
	}

## Adding Functionality

It's time to add some functionality to the module. The module is going to greet the user with a generic message. At this point, it's not the functionality that's interesting but how the pieces fit together.

The first file to edit is `/bin/hello`.

**/bin/hello**
	
	#!/usr/bin/env node
	
	var hello = require('./../lib/hello.js');
	hello.greet();

 This file has two jobs:
 
### 1. Specify Environment

If the file contains javascript code, which this file does, it needs to instruct the computer to execute this file with node. This is what the first line of the file does, and it is called the "shebang".

	#!/usr/bin/env node

Prefaced with "#!", the shebang provides the location of the program which should execute the files contents. In this case, the rest of the file is passed into node and is run as javascript.

### 2. Access Library

Fo the sake of simplicity, `/bin/hello` intentionally contains very little. The second line is in charge of starting the library script. It uses a `require` statement and passes in the path to the library.

	var hello = require('./../lib/hello.js');
	hello.greet();

-----

The only other file is `/lib/hello.js` which contains the code to execute. 

**/lib/hello.js**

	var hello = {};
	
	// greet the user with a generic message
	hello.greet = function () {
		process.stdout.write('Hello World!\r\n');
	};
	
	module.exports = hello;

`hello.js` exposes functionality that our command line tool can access. Currently, this is only the `greet` method.

## Installing and Executing

To install the module, navigate to the projects root directory in the terminal and run:

	npm install -g ./

`npm install` lets you install a new package. The `-g` flag tells npm to install the module globally. `./` points to the current directory where npm will look for the `package.json` file that you created.

Once you are returned to a new prompt, you package is installed! You will need to restart your terminal to be able to run the command.

You can now run `hello` from the command line, and you should see something like this:

	PHLMLAMEAD:~/code/hello$ hello
	Hello World!
	PHLMLAMEAD:~/code/hello$ _

## Uninstalling

Uninstalling any npm module is easy. To uninstall our hello module, run:

	npm uninstall -g hello

You need to include the `-g` flag if you originally installed the module globally. You also need to provide the name of the module to uninstall.

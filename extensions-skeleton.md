# Extensions Skeleton

After setting up a working development environment you are ready to start
developing. PHP provides a script that can generate a working source
tree on which to start developing your extension, saving you from the 
tedious task of preparing everything up in order to compile with the PHP
build system. This script is located on php-src/ext. There are 2 flavors
of the script one named ext_skel for unix environments developed using 
shell scripting and that can be executed like:

	./ext_skel
	
The other script is named ext_skel_win32.php programmed in php to 
compensate for the lack of advanced batch scripting on the windows
platform. It works similar to its unix counterpart with the difference
that you will need to install some dependencies to make it work. For
more details open the ext_skel_win32.php on a text editor and read the 
comments on it of how to use it.

If you execute the script without parameters you will get the available
flags to use. The output of the script is something as follows:

------------------------------------------------------------------------
./ext_skel --extname=module [--proto=file] [--stubs=file] [--xml[=file]]
           [--skel=dir] [--full-xml] [--no-help]

  --extname=module   module is the name of your extension
  --proto=file       file contains prototypes of functions to create
  --stubs=file       generate only function stubs in file
  --xml              generate xml documentation to be added to phpdoc-cvs
  --skel=dir         path to the skeleton directory
  --full-xml         generate xml documentation for a self-contained extension
                     (not yet implemented)
  --no-help          don't try to be nice and create comments in the code
                     and helper functions to test if the module compiled
------------------------------------------------------------------------

So lets say that you execute it as './ext_skel --extname=myextension',
this will generate a directory called "myextension" and inside that
directory you will find the following:

	tests
		Directory that holds a series of php scripts to make sure the
		extension is providing the functionality as it should.
		
	config.m4
		Autoconf script with the instructions needed to build the 
		extension on unix evironments.
		
	config.w32
		JScript with the instructions needed to build the extension 
		on windows evironment.
		
	CREDITS
		A list of developers seperated by comma used to generate the
		thanks information when building PHP.
		
	EXPERIMENTAL
		Serves as a flag to indicate that the extension is alpha or
		in planning stage.
		
	myextension.c
		The actual C source code for the extension. Module initialization
		code goes here as actual functions implementation.
		
	myextension.php
		Runs the extension php test's to see if everything is working
		as it should.
		
	php_myextension.h
		Declarations or prototypes of functions or methods.
	
For more information on how to use the ext_skel script please refer to
README.EXT_SKEL on the root dir of the PHP source tree. It explains all
aspects of the script as using a definition file to automatically
generate source code for function definitions etc...

	NOTE: While the ext_skel script is an easy way to rapidly have a
	working extension source tree, it hasn't been maintained over the
	years. It may generate deprecated code, so use with cautious!
    
------------------------------------------------------------------------
<table style="width: 100%;">
    <tr>
        <td align="left" width="33%">
            <a href="building-php-mac.md">&lt;&lt; Building on Mac</a>
        </td>
        
        <td align="center" width="33%">
            <a href="README.md">Table of Contents</a>
        </td>
        
        <td align="right" width="33%">
            <a href="exntensions-m4.md">Unix configuration file &gt;&gt;</a>
        </td>
    </tr>
</table>

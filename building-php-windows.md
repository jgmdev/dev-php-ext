# Windows
========================================================================

PHP building system on Windows almost emulates or mimics the autotools
system used on unix systems to build software. This is done by a set of
JScript files that define a lot of helper functions in combination of 
bat files for executing them. You will have the .bat counterpart of a sh
script, like for example configure.bat. With that said, configuring a 
build of PHP on windows should be pretty similar to doing it on a Unix 
system. The tricky part is setting the development environment.

	NOTE: You will find the functionality needed to write a working
	config.w32 for your extension on php-src/win32/build/confutils.js
	This file is automatically appended to the generated configure.js 
	file by buildconf.bat

On Windows PHP relies on Microsoft Compilers, other compilers such as
MingW or Clang aren't supported. Fortunately Microsoft has released a
set of free tools to develop on the Windows operating system. 

## Requirements
========================================================================

Here is what you will need to install in order to successfully compile 
PHP for windows:
	
a) Windows SDK 6.1
----------------------------------
	
Microsoft distributes the software development kit in two forms, 
Web Setup or DVD ISO. You can get either of those from the 
following:
	
	* Web Setup
	  http://www.microsoft.com/download/en/details.aspx?displaylang=en&id=11310


	* DVD ISO
	  http://www.microsoft.com/downloads/details.aspx?FamilyId=F26B1AA4-741A-433A-9BE5-FA919850BDBF&displaylang=en

b) Visual C++ 2008 Express Edition
----------------------------------
	
Visual Studio 2008 is break into several components each giving support 
to a specific language as the .Net platform. For PHP we only need the 
Visual C++ version that you can get from the following link:

	http://www.microsoft.com/visualstudio/en-us/products/2008-editions/express
	
c) PHP 5 binary tools
----------------------------------

The PHP team provides a set of ready to use packages with the tools
and dependencies needed to build on windows. You will find them at
the following link:
		
	http://windows.php.net/downloads/php-sdk/
		
At time of writing there are two versions of the binary tools:
		
	php-sdk-binary-tools-20110512.zip
	php-sdk-binary-tools-20110915.zip
		
So download the latest version as it should work with either PHP 5.3 
or 5.4 and follow the Steps to Build that explain how to use this file.
	
## Steps to Build
========================================================================

By now you should have everything needed to start so here is the list 
of steps needed:
	
a) Make the directory that will hold a working build system for PHP: 

	mkdir c:\php-sdk
	
b) Extract the binary tools inside the directory c:\php-sdk leaving 
you with:

	c:\php-sdk\bin
	c:\php-sdk\script
		
c) Open the Windows SDK 6.1 CMD Shell and do the following:
	
	setenv /release /x86 /xp
	cd c:\php-sdk\
	bin\phpsdk_setvars.bat
	bin\phpsdk_buildtree.bat php53dev
		
d) Extract the php source files and place them in:

	c:\php-sdk\php53dev\vc9\x86\php5.x.x (x.x stand for actual sub-version)
		
e) Compile the sources

	cd c:\php-sdk\php53dev\vc9\x86\php5.x.x
	buildconf
	configure --disable-all --enable-cli --enable-debug
	nmake


After running  nmake you should end with a bare bone debugging version 
of PHP that should serve you well for developing. For more configure
options run:

	configure --help
	
Keep in mind that some of the extensions have external dependencies that
you should download and extract on the deps folder accompanying the 
php5.x.x directory.

More details can be found at:

	http://www.php.net/manual/en/install.windows.php
	https://wiki.php.net/internals/windows/stepbystepbuild
    
------------------------------------------------------------------------
<table style="width: 100%;">
    <tr>
        <td align="left" width="33%">
            <a href="building-php-linux.md">&lt;&lt; Building on Linux</a>
        </td>
        
        <td align="center" width="33%">
            <a href="README.md">Table of Contents</a>
        </td>
        
        <td align="right" width="33%">
            <a href="building-php-mac.md">Building on Mac &gt;&gt;</a>
        </td>
    </tr>
</table>

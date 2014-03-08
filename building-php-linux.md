# Building on Linux

There are two ways on which you can get everything needed to start
developing on Linux. Use pre-compiled packages provided by your preferred
Linux distribution or compile PHP from source.

Most of the existing Linux distributions have ready to use packages to
start developing PHP extensions. On Debian based distributions this
package is usually named php5-dev, while on RPM based distributions
like opensuse it is named php5-devel.
	
	Installing on debian based distribution:
	
		sudo apt-get install php5-dev

	Installing on RPM based distributions:
	
		su && yum install php5-devel
	
Installing the package provided by your Linux distribution should 
also take care of installing all the dependencies needed to compile PHP 
extensions. The only drawback is that when developing PHP extensions 
you will want a PHP binary that includes debugging symbols, so compiling
from source with the --enable-debug flag would be the best. Fortunately 
some Linux distributions also provide a package to install PHP binaries
with unstripped debugging symbols.

	Installing debugging symbols on Debian based distribution:
	
		sudo apt-get install php5-dbg
		
	This will install debugging binaries at:
	
		/usr/lib/debug/usr/bin/
		
If your Linux distribution don't ships with PHP debugging packages or
if you want to develop with the most recent version of PHP, you will 
have to compile your own version. The dependencies or requirements 
needed to build are:

	a) autoconf: 2.13+ (for PHP < 5.4.0), 2.59+ (for PHP >= 5.4.0)
	b) automake: 1.4+
	c) libtool: 1.4.x+ (except 1.4.2)
	d) re2c: Version 0.13.4 or newer
	e) flex: Version 2.5.4 (for PHP <= 5.2)
	f) bison: Version 1.28 (preferred), 1.35, or 1.75
	
The steps to build on Linux should be something like follows:

	tar -xvz php-5.x.x.tar.gz
	cd php-5.x.x
	./configure --disable-all --enable-cli --enable-debug
	make
	make install
	
To get more configuration options you can run:

	./configure --help

More details about building on Linux/Unix environments can be found at:	

	http://www.php.net/manual/en/install.unix.php
    
------------------------------------------------------------------------
<table style="width: 100%;">
    <tr>
        <td align="left" width="33%">
            <a href="building-php.md">&lt;&lt; Building PHP</a>
        </td>
        
        <td align="center" width="33%">
            <a href="README.md">Table of Contents</a>
        </td>
        
        <td align="right" width="33%">
            <a href="building-php-windows.md">Building on Windows &gt;&gt;</a>
        </td>
    </tr>
</table>

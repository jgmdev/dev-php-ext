<table style="width: 100%;">
    <tr>
        <td align="left" width="33%">
            <a href="introduction.md">&lt;&lt; Introduction</a>
        </td>
        
        <td align="center" width="33%">
            <a href="README.md">Table of Contents</a>
        </td>
        
        <td align="right" width="33%">
            <a href="building-php-linux.md">Linux &gt;&gt;</a>
        </td>
    </tr>
</table>
------------------------------------------------------------------------

# Building PHP
========================================================================

Before you start developing a PHP extension the first thing you would need
is the PHP source. You could get it by visiting http://php.net/downloads.php

You will need a C or C++ compiler depending on the type of extension you
are going to develop. For compiling PHP sources a C compiler is just fine,
since PHP is developed in pure C including the extensions shipped with it.
If you are going to develop a PHP extension that wraps/binds a C++ library
then you should install a C++ compiler as well.

	NOTE: PHP configure script uses the flag --enable-debug to include 
	debugging symbols on the resulting binaries. It's strongly suggested
	to use this flag, since it will help later if you have to do some 
	debugging with tools like valgrind and gdb.

------------------------------------------------------------------------
<table width="100%">
    <tr>
        <td align="left" width="33%">
            
        </td>
        
        <td align="center" width="33%">
            <a href="README.md">Table of Contents</a>
        </td>
        
        <td align="right" width="33%">
            <a href="">Building PHP &gt;&gt;</a>
        </td>
    </tr>
</table>

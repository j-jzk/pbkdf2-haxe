PBKDF2 for Haxe
===============

PBKDF2 (Password-Based Key Derivation Function 2) is a key derivation function that you can read all about [on Wikipedia](http://en.wikipedia.org/wiki/PBKDF2).

Basically, it lets you do a hash similar to MD5 or SHA1, but with a salt, and it can repeat thousands of times to make it harder to crack - the longer it takes to generate the key the longer it will take to crack.  Anyway, it is a good way to store user passwords.  Even big frameworks like Django use it by default.

This is the version updated for Haxe 4.0 (throws no _deprecated_ warnings on PHP); the [original repository](https://github.com/jasononeil/PBKDF2-Haxe/) is unmaintained.

Installation
------------

To install:

	haxelib git pbkdf2 https://github.com/j-jzk/pbkdf2-haxe.git

And then in your project's hxml build file, add

    --lib pbkdf2

Usage
-----

```
var str = PBKDF2.encode(pass, salt, numIterations, numBytes);
```

It's best to use a different salt for each unique password... Here I use the helper from the [Random](https://github.com/jasononeil/hxrandom) haxelib.

```
user = new User();
user.username = "jason";
user.salt = Random.string(20);
user.password = PBKDF2.encode("password", user.salt, 1000, 20);
```

### Asynchronous Usage

On targets that support haxe.Timer (flash8, flash, js, cs, java) you can use the Async version:

```
var onComplete = function(str:String) {
	trace ("Hash is " + str);
}

PBKDF2.encodeAsync("password", "salt", 1000, 20, onComplete);
```

Platform Support
----------------

Currently tested on Flash9, JS, CPP.  Works on PHP, but our test suite doesn't run.  I did some manual tests to verify though.

Neko only works with v2.0, so you'll need to install the release candidate or latest SVN.  You'll also need Haxe3 (latest SVN), and you will need to use the `-D neko_v2` command line option.  It will not work on Neko v1.8, because the algorithm relies heavily on Bitwise Integer operations and neko v1 does not support 32bit Integers, so a lot of things were broken that I have not debugged.  
*Currently, neko does work but runs incredibly slowly - 60 times slower than JS and 120 times slower than CPP.  Some profiling will definitely be needed.*

No idea if it works on Java or C#, but quite possibly - if their Integer operations and arrays behave similarly enough.

Does not work on Flash8 (generates different numbers, have not debugged yet).  

Attribution & Licence
---------------------

This is a copy-of-a-copy-of-a-derivative, and a separate port:

1. The Javascript version: Copyright (c) 2007, Parvez Anandam  
parvez.anandam@cern.ch  
http://anandam.name/pbkdf2  
(Uses Paul Johnston's excellent SHA-1 JavaScript library sha1.js)  
Thanks to Felix Gartsman for pointing out a bug in version 1.0  
BSD License

2.  The AS3 version: http://code.google.com/p/as3-pbkdf2/  
jeesmon@gmail.com  
BSD License

3. Haxe implementation by Jason O'Neil  
https://github.com/jasononeil/PBKDF2-Haxe  
BSD license

4. The PHP implementation was derived from:  
https://defuse.ca/php-pbkdf2.htm havoc AT defuse.ca  
Released under Public Domain.  Haxe translation by Jason O'Neil.

5. Library modified for Haxe 4 compatibility by _postite_,  
repackaged by _j-jzk_ (original repository was unmaintained).

See the LICENSE.txt file for the full license.

Making Changes
--------------

 * Edit src/PBKDF2.hx
 * Add unit tests to test/PBKDF2Test.hx
 * `haxelib run munit t` - run unit tests, check all are passing
 * `haxelib run mlib v` - increment version number
 * `haxelib run mlib submit` - submit to Haxelib

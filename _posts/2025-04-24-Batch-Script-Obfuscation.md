---
layout: post
title: Batch Script Obfuscation
subtitle: Obfuscation techniques seen in the wild
cover-img: /assets/img/honk.png
thumbnail-img: /assets/img/bat.png
share-img: /assets/img/bat.png
tags: []
---

Earlier this week, I was investigating a batch script that made its way onto someone's system. I enjoy deobfuscating files, so here are some things specific to (but maybe not exclusive to) batch file obfuscation that I saw.

Below is the first part of the batch file. Essentially what was going on was there was a bunch of junk data that was inserted between some variables / text. I replaced the real junk to make it more visible, but the original junk data was a lot more randomized and less apparent that it repeated. 
~~~
@echo off
set s=%asdfasdfasdfasdfasdfasdfasdfasdfasdfasdfasdfasdfasdf%s%asdfasdfasdfasdfasdfasdfasdfasdfasdfasdfasdfasdfasdf%e%asdfasdfasdfasdfasdfasdfasdfasdfasdfasdfasdfasdfasdf%t%asdfasdfasdfasdfasdfasdfasdfasdfasdfasdfasdfasdfasdf%l%asdfasdfasdfasdfasdfasdfasdfasdfasdfasdfasdfasdfasdf%o%asdfasdfasdfasdfasdfasdfasdfasdfasdfasdfasdfasdfasdf%c%asdfasdfasdfasdfasdfasdfasdfasdfasdfasdfasdfasdfasdf%a%asdfasdfasdfasdfasdfasdfasdfasdfasdfasdfasdfasdfasdf%l%asdfasdfasdfasdfasdfasdfasdfasdfasdfasdfasdfasdfasdf%
set e=%asdfasdfasdfasdfasdfasdfasdfasdfasdfasdfasdfasdfasdf%e%asdfasdfasdfasdfasdfasdfasdfasdfasdfasdfasdfasdfasdf%n%asdfasdfasdfasdfasdfasdfasdfasdfasdfasdfasdfasdfasdf%a%asdfasdfasdfasdfasdfasdfasdfasdfasdfasdfasdfasdfasdf%b%asdfasdfasdfasdfasdfasdfasdfasdfasdfasdfasdfasdfasdf%
set d=%asdfasdfasdfasdfasdfasdfasdfasdfasdfasdfasdfasdfasdf%l%asdfasdfasdfasdfasdfasdfasdfasdfasdfasdfasdfasdfasdf%e%asdfasdfasdfasdfasdfasdfasdfasdfasdfasdfasdfasdfasdf%d%asdfasdfasdfasdfasdfasdfasdfasdfasdfasdfasdfasdfasdf%e%asdfasdfasdfasdfasdfasdfasdfasdfasdfasdfasdfasdfasdf%l%asdfasdfasdfasdfasdfasdfasdfasdfasdfasdfasdfasdfasdf%a%asdfasdfasdfasdfasdfasdfasdfasdfasdfasdfasdfasdfasdf%y%asdfasdfasdfasdfasdfasdfasdfasdfasdfasdfasdfasdfasdf%e%asdfasdfasdfasdfasdfasdfasdfasdfasdfasdfasdfasdfasdf%d%asdfasdfasdfasdfasdfasdfasdfasdfasdfasdfasdfasdfasdf%
set x=%asdfasdfasdfasdfasdfasdfasdfasdfasdfasdfasdfasdfasdf%e%asdfasdfasdfasdfasdfasdfasdfasdfasdfasdfasdfasdfasdf%x%asdfasdfasdfasdfasdfasdfasdfasdfasdfasdfasdfasdfasdf%p%asdfasdfasdfasdfasdfasdfasdfasdfasdfasdfasdfasdfasdf%a%asdfasdfasdfasdfasdfasdfasdfasdfasdfasdfasdfasdfasdf%n%asdfasdfasdfasdfasdfasdfasdfasdfasdfasdfasdfasdfasdf%s%asdfasdfasdfasdfasdfasdfasdfasdfasdfasdfasdfasdfasdf%i%asdfasdfasdfasdfasdfasdfasdfasdfasdfasdfasdfasdfasdf%o%asdfasdfasdfasdfasdfasdfasdfasdfasdfasdfasdfasdfasdf%n%asdfasdfasdfasdfasdfasdfasdfasdfasdfasdfasdfasdfasdf%
set rah=%s% %e%%d%%x%
call %rah%
~~~

From there, if we trim it down, we get this

~~~
@echo off
set s=setlocal
set e=enab
set d=ledelayed
set x=expansion
set rah=s edx
call rah
~~~

Which further evaluates to

~~~
@echo off
setlocal enabledelayedexpansion
~~~

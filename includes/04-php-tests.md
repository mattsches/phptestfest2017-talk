## Tests für den PHP-Quellcode

* PHP ist in C geschrieben <!-- .element: class="fragment" -->
* Testsuite mit funktionalen Tests <!-- .element: class="fragment" -->
* Die Tests für PHP sind in PHP geschrieben<!-- .element: class="fragment" -->
* => keine C-Kenntnisse nötig! <!-- .element: class="fragment" -->
* (aber sie helfen schon 😀) <!-- .element: class="fragment" -->



## Normalerweise

würde man so vorgehen:



#### PHP-Source klonen

```
$ git clone https://github.com/php/php-src.git
Klone nach 'php-src'...
remote: Counting objects: 704994, done.
remote: Compressing objects: 100% (21/21), done.
remote: Total 704994 (delta 5), reused 5 (delta 0), pack-reused 704973
Empfange Objekte: 100% (704994/704994), 285.98 MiB | 942.00 KiB/s, Fertig.
Löse Unterschiede auf: 100% (546203/546203), Fertig.
```



#### PHP kompilieren

```
$ time make -j5
...
Build complete.
Don't forget to run 'make test'.
real	1m36,147s   <====
user	5m2,527s
sys	0m26,287s
```



#### Kompilat prüfen

```
$ sapi/cli/php --version
PHP 7.2.0-dev (cli) (built: Jul 14 2017 10:16:10) ( ZTS DEBUG )
Copyright (c) 1997-2017 The PHP Group
Zend Engine v3.2.0-dev, Copyright (c) 1998-2017 Zend Technologies
```



#### Testsuite ausführen

```
$ time make test
... 11301 Tests ...
=====================================================================
FAILED TEST SUMMARY
---------------------------------------------------------------------
bool checkdnsrr ( string $host [, string $type = "MX" ] ); [ext/standard/tests/checkdnsrr.phpt]
array dns_get_record ( string $hostname [, int $type = DNS_ANY [, array &$authns [, array &$addtl [, bool &$raw = false ]]]] ); [ext/standard/tests/dns_get_record.phpt]
bool dns_get_mx ( string $hostname , array &$mxhosts [, array &$weight ] ); [ext/standard/tests/network/dns_get_mx.phpt]
dns_get_record() CAA tests [ext/standard/tests/network/dns_get_record_caa.phpt]
getmxrr() test [ext/standard/tests/network/getmxrr.phpt]
=====================================================================

You may have found a problem in PHP.
This report can be automatically sent to the PHP QA team at
http://qa.php.net/reports and http://news.php.net/php.qa.reports
This gives us a better understanding of PHP's behavior.
If you don't want to send the report immediately you can choose
option "s" to save it.	You can then email it to qa-reports@lists.php.net later.
Do you want to send this report now? [Yns]: 

real	6m15,913s
```



#### Einzelnen Test ausführen

```bash
$ sapi/cli/php run-tests.php -P tests/basic/001.phpt

=====================================================================
PHP         : /home/mattsches/Projekte/php-src/sapi/cli/php 
PHP_SAPI    : cli
PHP_VERSION : 7.3.0-dev
ZEND_VERSION: 3.3.0-dev
PHP_OS      : Linux - Linux falcon 4.12.10-041210-generic #201708300614 SMP Wed Aug 30 10:16:40 UTC 2017 x86_64
INI actual  : /home/mattsches/Projekte/php-src
More .INIs  :   
---------------------------------------------------------------------
PHP         : /home/mattsches/Projekte/php-src/sapi/phpdbg/phpdbg 
PHP_SAPI    : phpdbg
PHP_VERSION : 7.3.0-dev
ZEND_VERSION: 3.3.0-dev
PHP_OS      : Linux - Linux falcon 4.12.10-041210-generic #201708300614 SMP Wed Aug 30 10:16:40 UTC 2017 x86_64
INI actual  : /home/mattsches/Projekte/php-src
More .INIs  : 
---------------------------------------------------------------------
CWD         : /home/mattsches/Projekte/php-src
Extra dirs  : 
VALGRIND    : Not used
=====================================================================
Running selected tests.
PASS Trivial "Hello World" test [tests/basic/001.phpt] 
=====================================================================
Number of tests :    1                 1
Tests skipped   :    0 (  0.0%) --------
Tests warned    :    0 (  0.0%) (  0.0%)
Tests failed    :    0 (  0.0%) (  0.0%)
Expected fail   :    0 (  0.0%) (  0.0%)
Tests passed    :    1 (100.0%) (100.0%)
---------------------------------------------------------------------
Time taken      :    0 seconds
=====================================================================
```
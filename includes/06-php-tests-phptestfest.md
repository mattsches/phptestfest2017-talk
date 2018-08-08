## Vorgehensweise fürs PHP Testfest 2017



#### Repository forken <i class="fa fa-code-fork" aria-hidden="true"></i>

Spezielles Source-Repository:

<i class="fa fa-github" aria-hidden="true"></i> [phpcommunity/phptestfest-php-src](https://github.com/phpcommunity/phptestfest-php-src)

```
$ git clone git@github.com:mattsches/phptestfest-php-src.git
Klone nach 'phptestfest-php-src'...
remote: Counting objects: 662117, done.
remote: Compressing objects: 100% (169/169), done.
remote: Total 662117 (delta 287), reused 308 (delta 233), pack-reused 661715
Empfange Objekte: 100% (662117/662117), 244.86 MiB | 388.00 KiB/s, Fertig.
Löse Unterschiede auf: 100% (516720/516720), Fertig.
```



#### docker-phpqa installieren 

Docker muss installiert sein, dann:

```bash
curl -s https://raw.githubusercontent.com/herdphp/docker-phpqa/master/bin/installer.sh | bash
```

Nicht zu verwechseln mit [phpqa](https://edgedesigncz.github.io/phpqa/)!!!

Note:
Letzteres ist ein ebenfalls Docker-basiertes Meta-Tool, das Tools wie

`phploc, phpcpd, phpcs, pdepend, phpmd, phpmetrics`

in aktuellen Versionen hinter einem Befehl (`phpaq`) versteckt.



#### Test generieren

```bash
$ phpqa generate tests -f ucfirst -b
$ cat tests/ucfirst_basic.phpt
--TEST--
Test function ucfirst() by calling it with its expected arguments
--FILE--
<?php

echo "*** Test by calling method or function with its expected arguments ***\n";

$str =

var_dump(ucfirst( $str ) );

?>
--EXPECTF--
```

Note:

Wer findet den Fehler? Klar, `$str` wurde nicht korrekt initialisiert.



#### Test ausführen

```bash
$ phpqa run tests/ucfirst_basic.phpt
71: Pulling from herdphp/phpqa
ad74af05f5a2: Already exists
8cd4c39cc1ca: Already exists
a5a61faae0a1: Already exists
d3c3b2c4e7f8: Already exists
9f5e32d10c89: Already exists
d5fb1262587e: Already exists
3fba5f5d8cd4: Already exists
Digest: sha256:1a5873c839ec8f950f011b04683ca7e8a07321109ceba05341c18730682396b0
Status: Image is up to date for herdphp/phpqa:71
/bin/bash /usr/src/php/libtool --silent --preserve-dup-deps --mode=install cp ext/opcache/opcache.la /usr/src/php/modules

Test build successfully.
=)

=====================================================================
PHP         : /usr/src/php/sapi/cli/php
PHP_SAPI    : cli
PHP_VERSION : 7.1.7
ZEND_VERSION: 3.1.0
PHP_OS      : Linux - Linux c4794693099a 4.9.41-moby #1 SMP Wed Sep 6 00:05:16 UTC 2017 x86_64
INI actual  : /usr/src/php/tmp-php.ini
More .INIs  :
---------------------------------------------------------------------
PHP         : /usr/src/php/sapi/phpdbg/phpdbg
PHP_SAPI    : phpdbg
PHP_VERSION : 7.1.7
ZEND_VERSION: 3.1.0
PHP_OS      : Linux - Linux c4794693099a 4.9.41-moby #1 SMP Wed Sep 6 00:05:16 UTC 2017 x86_64
INI actual  : /usr/src/php/tmp-php.ini
More .INIs  :
---------------------------------------------------------------------
CWD         : /usr/src/php
Extra dirs  :
VALGRIND    : Not used
=====================================================================
Running selected tests.
FAIL Test function ucfirst() by calling it with its expected arguments [/usr/src/phpt/ucfirst_basic.phpt]
=====================================================================
Number of tests :    1                 1
Tests skipped   :    0 (  0.0%) --------
Tests warned    :    0 (  0.0%) (  0.0%)
Tests failed    :    1 (100.0%) (100.0%)
Expected fail   :    0 (  0.0%) (  0.0%)
Tests passed    :    0 (  0.0%) (  0.0%)
---------------------------------------------------------------------
Time taken      :    0 seconds
=====================================================================

=====================================================================
FAILED TEST SUMMARY
---------------------------------------------------------------------
Test function ucfirst() by calling it with its expected arguments [/usr/src/phpt/ucfirst_basic.phpt]
=====================================================================
```



#### Test gegen verschiedene PHP-Versionen ausführen

```
$ phpqa run phptest/ucfirst_basic.phpt # runs against PHP7.1
$ phpqa run phptest/ucfirst_basic.phpt 72 # runs against PHP7.2
$ phpqa run phptest/ucfirst_basic.phpt 56 # runs against PHP5.6
$ phpqa run phptest/ucfirst_basic.phpt 70 # runs against PHP7.0
```

Note:

Offenbar ein Feature von `docker-phpqa`, nicht selbst getestet.



#### Fehlgeschlagenen Test debuggen

```
$ ls tests/71
ucfirst_basic.diff    ucfirst_basic.log    ucfirst_basic.php    ucfirst_basic.sh
ucfirst_basic.exp    ucfirst_basic.out    ucfirst_basic.phpt
```

Note:

Sehen wir uns eins der automatisch generierten Files einmal genauer an: `ucfirst_basic.out`



#### ucfirst_basic.out

```
*** Test by calling method or function with its expected arguments ***

Notice: Undefined variable: str in /usr/src/phpt/ucfirst_basic.php on line 10
string(0) ""
```

Note:

Das ist die Ausgabe, die anstelle des erwarteten Resultats zurückgegeben wurde



#### Fehlgeschlagenen Test fixen

```
--TEST--
Test function ucfirst() by calling it with its expected arguments
--FILE--
<?php
echo "*** Test by calling method or function with its expected arguments ***\n";
$str = 'test with whitespace';
var_dump(ucfirst($str));
?>
--EXPECTF--
*** Test by calling method or function with its expected arguments ***
string(20) "Test with whitespace"
```



#### Yes, we fixed it! <i class="fa fa-wrench" aria-hidden="true"></i>

```bash
$ phpqa run phptest/ucfirst_basic.phpt
Test build successfully.
=)

=====================================================================
PHP         : /usr/src/php/sapi/cli/php
PHP_SAPI    : cli
PHP_VERSION : 7.1.7
ZEND_VERSION: 3.1.0
PHP_OS      : Linux - Linux e1e0e2b5c5a2 4.9.41-moby #1 SMP Wed Sep 6 00:05:16 UTC 2017 x86_64
INI actual  : /usr/src/php/tmp-php.ini
More .INIs  :
---------------------------------------------------------------------
PHP         : /usr/src/php/sapi/phpdbg/phpdbg
PHP_SAPI    : phpdbg
PHP_VERSION : 7.1.7
ZEND_VERSION: 3.1.0
PHP_OS      : Linux - Linux e1e0e2b5c5a2 4.9.41-moby #1 SMP Wed Sep 6 00:05:16 UTC 2017 x86_64
INI actual  : /usr/src/php/tmp-php.ini
More .INIs  :
---------------------------------------------------------------------
CWD         : /usr/src/php
Extra dirs  :
VALGRIND    : Not used
=====================================================================
Running selected tests.
PASS Test function ucfirst() by calling it with its expected arguments [/usr/src/phpt/ucfirst_basic.phpt]
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

Note:

Wir bekommen kein `FAIL` mehr, sondern ein `PASS`

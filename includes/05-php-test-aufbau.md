## Aufbau eines PHP-Tests

```bash
tests/basic/001.phpt
```

Note:

Man beachte die Dateiendung `.phpt`


<section data-transition="none">
    <h4>Aufbau eines PHP-Tests</h4>
    <pre><code data-trim data-noescape>
                --TEST--
                Trivial "Hello World" test
                --FILE--
                &lt;?php echo "Hello World"?>
                --EXPECT--
                Hello World

            </code></pre>
</section>



<section data-transition="none">
    <h4>Aufbau eines PHP-Tests</h4>
    <pre><code data-trim data-noescape>
                <mark>--TEST--
                Trivial "Hello World" test</mark>
                --FILE--
                &lt;?php echo "Hello World"?>
                --EXPECT--
                Hello World

            </code></pre>
</section>



<section data-transition="none">
    <h4>Aufbau eines PHP-Tests</h4>
    <pre><code data-trim data-noescape>
                --TEST--
                Trivial "Hello World" test
                <mark>--FILE--
                &lt;?php echo "Hello World"?></mark>
                --EXPECT--
                Hello World

            </code></pre>
</section>



<section data-transition="none">
    <h4>Aufbau eines PHP-Tests</h4>
    <pre><code data-trim data-noescape>
            --TEST--
            Trivial "Hello World" test
            --FILE--
            &lt;?php echo "Hello World"?>
            <mark>--EXPECT--
            Hello World</mark>

        </code></pre>
</section>



#### Mehr Details?

Dokumentation [qa.php.net](https://qa.php.net/phpt_details.php)

```
--TEST--
[--DESCRIPTION--]
[--CREDITS--]
[--SKIPIF--]
[--REQUEST--]
[--POST-- | --PUT-- | --POST_RAW-- | --GZIP_POST-- | --DEFLATE_POST-- | --GET--]
[--COOKIE--]
[--STDIN--]
[--INI--]
[--ARGS--]
[--ENV--]
--FILE-- | --FILEEOF-- | --FILE_EXTERNAL-- | --REDIRECTTEST--
[--HEADERS--]
[--CGI--]
[--XFAIL--]
[--EXPECTHEADERS--]
--EXPECT-- | --EXPECTF-- | --EXPECTREGEX-- | --EXPECT_EXTERNAL-- | --EXPECTF_EXTERNAL-- | --EXPECTREGEX_EXTERNAL-- 
[--CLEAN--]
```

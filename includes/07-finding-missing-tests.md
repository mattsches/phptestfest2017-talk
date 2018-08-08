## Fehlende Tests finden <i class="fa fa-search" aria-hidden="true"></i>



#### [Bugtracker]([bugs.php.net](https://bugs.php.net) <i class="fa fa-bug" aria-hidden="true"></i>

* nach interessanten Bugs durchsuchen.
* Gibt es schon einen Test dafür?
* Wenn nicht, einen schreiben und eine `--XFAIL--` Sektion hinzufügen

Note:

* --XFAIL--: "This section identifies this test as one that is currently expected to fail. It should include a brief description of why it's expected to fail."
* Es geht erstmal um Test Coverage, nicht darum, den Code zu fixen



#### [Test and Code Coverage Analysis](http://gcov.php.net/) <i class="fa fa-code" aria-hidden="true"></i>

* nach Code ohne Test-Coverage durchsuchen.
* Versuchen, den C-Code zu verstehen 😜
* Einen dafür Test schreiben.

Note:

* Ggf. hilft es, nach bestimmten Ausdrücken zu greppen (`function(`)
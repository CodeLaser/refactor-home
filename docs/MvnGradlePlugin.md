
The Maven and Gradle plugins
============================

Editing your `pom.xml` file
---------------------------


Editing your Gradle files
-------------------------

We detail the situation here for `.kts` files. 
First, ensure that the


The `copy` goal/task
--------------------

```shell
 mvn io.codelaser:codelaser-refactor-mvnplugin:refactor-copy
```

or 

```shell
gradle  :some-sub-project:copy
```


The `generate-docstrings` goal/task
-----------------------------------

```shell 
 mvn io.codelaser:codelaser-refactor-mvnplugin:generate-docstrings
```

```shell
gradle generate-docstrings
```


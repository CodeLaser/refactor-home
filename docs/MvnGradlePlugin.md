The Maven and Gradle plugins
============================

The Maven and Gradle plugins behave very similarly, and require essentially the same input, albeit in different formats:

* the location of CodeLaser's plugin repository
* activation of the plugin, and parameters to the plugin
* a reference to the Java JAR for the docstring annotations
* a CodeArtifact auth token, which is generally valid for 24h only, so it must be read from a secure place that is easy
  to update.

Maven: editing the `pom.xml` file
---------------------------------

Add the following block to the main section (or add to the existing `pluginRepositories` section, if you already have
one):

```xml

<pluginRepositories>
    <pluginRepository>
        <id>CodeLaser-public</id>
        <name>Public CodeLaser repository</name>
        <url>https://codelaser-975050168225.d.codeartifact.eu-central-1.amazonaws.com/maven/CodeLaser-public</url>
    </pluginRepository>
</pluginRepositories>
```

Add the following block to the `pluginManagement/plugins` section (or create one, if you don't have one already):

```xml

<plugin>
    <groupId>io.codelaser</groupId>
    <artifactId>codelaser-refactor-mvnplugin</artifactId>
    <version>1.0.0</version>
    <configuration>
        <projectsDir>${env.REFACTOR_HOME}/projects</projectsDir>
        <projectName>your-project-name</projectName>
        <modificationAnalysis>false</modificationAnalysis>
        <docker>true</docker>
        <branch>master</branch>
    </configuration>
</plugin>
```

Add the following dependency to your normal dependency section:

```xml

<dependency>
    <groupId>io.codelaser</groupId>
    <artifactId>codelaser-metrics-support</artifactId>
    <version>1.0.0</version>
</dependency>
```

You may have to add a few more dependencies because the refactor server's bytecode parsing system is not as laid back as
the Java runtime in terms of missing code.

You'll need a `settings.xml` file in your local `~/.m2` repository:

```xml

<settings>
    <servers>
        <server>
            <id>CodeLaser-public</id>
            <username>aws</username>
            <password>${env.CODEARTIFACT_AUTH_TOKEN}</password>
        </server>
    </servers>
</settings>
```

This setup assumes that the environment variable `CODEARTIFACT_AUTH_TOKEN` points to the authorization token obtained
for CodeLaser's CodeArtifact repository.


Editing Gradle files
--------------------

We detail the situation here for `.kts` files; Groovy versions are very similar.

A `pluginManagement` block in the`settings.gradle.kts` file defines the location for CodeLaser's CodeArtifact
repository:

```kotlin
pluginManagement {
    val codeartifactToken: String by settings

    repositories {
        maven {
            url = uri("https://codelaser-975050168225.d.codeartifact.eu-central-1.amazonaws.com/maven/CodeLaser-public")
            credentials {
                username = "aws"
                password = codeartifactToken
            }
        }
    }
}
```

This assumes that the Gradle property `codeartifactToken` is set in one of your gradle property files, or passed on on
the command line. Alternatively, the password can be read from an environment variable, using

```kotlin
password = System.getenv("CODEARTIFACT_AUTH_TOKEN")
```

The `plugins` section of `build.gradle.kts`:

```kotlin
plugins {
    java
    id("io.codelaser.jfocus.refactor-plugin").version("1.0.0")
}
```

In the dependency block:

```kotlin
    implementation("io.codelaser:codelaser-metrics-support:1.0.0")
```

Finally, `refactor` options block of `build.gradle.kts`:

```kotlin
refactor {
    projectName = "test-viewer"
    projectsDir = "$refactorHome/projects"
    docker = true
    apiKeyEnvVar = "ANTHROPIC_KEY"
    provider = "anthropic"
    modelName = "claude-3-7-sonnet-20250219"
}
```

Here, we're assuming a Gradle property called `refactorHome`.

The `copy` goal/task
--------------------

```shell
 mvn io.codelaser:codelaser-refactor-mvnplugin:refactor-copy
```

or

```shell
gradle  :some-sub-project:copy
```

Options:

* `docker`: default true, indicates that the refactor server resides in a Docker image. The effect is that the starting
  point for the relative paths in the input configuration is `/home/ubuntu` rather than `$REFACTOR_HOME`.
* `projectsDir`
* `modificationAnalysis`
* `branch`: the Git branch that the refactor server will default at startup, or after a reset

The copy task will generate an `inputConfiguration.json` file, by default written to the `target` (mvn) or `build/` (
gradle) directory.

The `generate-docstrings` goal/task
-----------------------------------

This task can be run repeatedly; it will not overwrite existing docstrings. It requires a few parameters, to point it to
an LLM it can use to send batches of source code to:

```shell 
 mvn io.codelaser:codelaser-refactor-mvnplugin:generate-docstrings \
    -DmaxInputSize=50001 -DmaxTypesInChunk=12\
    -Dprovider=anthropic -DapiKeyEnvVar=ANTHROPIC_KEY -DmodelName=claude-3-7-sonnet-20250219
```

The properties `maxInputSize` and `maxTypesInChunk` can be adjusted upwards when the LLM has a greater input window,
respectively upwards when it is more reliable at performing sequenced tasks.

Setting the properties in the `build.gradle.kts` file:

```kotlin
refactor {
    projectName = "test-viewer"
    projectsDir = "$refactorHome/projects"
    docker = true
    apiKeyEnvVar = "ANTHROPIC_KEY"
    provider = "anthropic"
    modelName = "claude-3-7-sonnet-20250219"
}
```

we can simply run

```shell
gradle generate-docstrings
```


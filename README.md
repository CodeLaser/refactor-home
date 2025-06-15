
CodeLaser refactor server
-------------------------

Components:

1. the `refactor-deploys` directory, to be cloned from CodeLaser's GitHub page. This is the README file for this directory.
2. a Docker image `codelaser/refactor-rest:latest`, hosted at Amazon AWS's Elastic Container Registry, which contains the refactoring server (a Java Micronaut application)
3. a Docker image `codelaser/refactor-mcp:latest`, which contains the MCP server (a Python application)
4. access to a Maven and/or Gradle plugin to install your project into the `refactor-deploys/projects` directory. The plugin is hosted on AWS's CodeArtifact.
5. access to a small support Jar that contains the annotations that hold the docstrings in your code. Also available on CodeArtifact.

Prerequisites:

1. ensure you have at least a JDK 17 installed, and `mvn` or `gradle` either installed or present in your project. The ref server will be referring to jars in your local Maven repository (typically `~/.m2`). The plugins assume that your project's source code management system is Git.
2. an `Ollama` running locally, with the "nomic-embed-text" embedding model loaded
3. Docker locally installed

Main installation steps:

1. clone `refactor-deploys`, and point the environment variable `REFACTOR_HOME` to it
2. set-up AWS credentials for CodeArtifact
3. modify your Maven's `pom.xml` or Gradle's `settings.gradle` (or `settings.gradle.kts`) and `build.gradle` (`build.gradle.kts`) files to activate the plugin, and set some parameters
4. run the plugin to generate Docstrings for your Java classes, using your favourite LLM. They are stored in your source code's `package-info.java`, and are computed for your local embedding server
5. run the plugin to copy your project to the `refactor-deploys/projects` directory. The plugin will write, among others, an `inputConfiguration.yml` file that describes all the project's dependencies for the refactor server to read
6. login to AWS ECR and pull the refactor server images
7. start the refactor server using the `compose.yml` file and `docker-compose`

How it works:

* the AI loads the MCP server.
* The MCP server calls the REST API of the refactor server.
* The refactor server reads the source code from the Git repo that has been copied.
* It makes modifications that are stored in separate branches.
* You instruct the AI to accept or reject these branches; or to reset to a given baseline situation.
* You decide about pushing or pulling the code held in the copy.
* The `copy` plugin must be used when the project's dependencies change.
* You decide when it is time to update the docstrings.

Main limitations

* only support for mvn, gradle
* only JUnit tests are executed during validation of a refactoring
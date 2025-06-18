Detailed installation instructions
==================================

Refactor server home directory
------------------------------

In your directory of choice, run

```shell
git clone https://github.com/CodeLaser/refactor-home.git
export REFACTOR_HOME="$PWD/refactor-home"
```

The environment variable `REFACTOR_HOME` will be used by the Maven/Gradle plugin to determine where to make a copy of
the source project.

Access to AWS CodeArtifact
--------------------------

```shell
export CODEARTIFACT_AUTH_TOKEN=`aws codeartifact get-authorization-token \
   --profile bart \
   --domain codelaser --domain-owner 975050168225 --region eu-central-1 \
   --query authorizationToken --output text`
```

Even though the CodeArtifact repo is publicly accessible, you must log in to AWS CodeArtifact, and present a token.

The `.aws/credentials` file looks like

```properties
[bart]
aws_access_key_id=AKIxxxxxxxxxxxx
aws_secret_access_key=UxbtVwwhYYYYYYyyYYyyYYyyYYYYYYYYY
```

Ensure that this credentials file is properly secured (`chmod 400`, for example).

Access to AWS Elastic Container Registry
----------------------------------------

Obtain a login password from AWS, and pass that on to the `docker` command:

```shell
aws ecr get-login-password --region eu-central-1 --profile bart | \
  docker login --username AWS --password-stdin \
     975050168225.dkr.ecr.eu-central-1.amazonaws.com
```

The images are named

```
975050168225.dkr.ecr.eu-central-1.amazonaws.com/codelaser/refactor-rest:202506131500
975050168225.dkr.ecr.eu-central-1.amazonaws.com/codelaser/refactor-mcp:202506131500
```

Starting the Docker images
--------------------------

The `compose.yml` file.

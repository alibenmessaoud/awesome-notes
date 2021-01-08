# jgitver

[jgitver](https://github.com/jgitver/jgitver.github.io) consists of a set of library and plugins allowing to automatically compute project versions based on:

- git history
- git tags (annotated & lightweight)
- git branches
- configuration (predefined or explicit)

#### project initialization

Open a docker container with all the required tooling: maven, git.

```docker run -it --rm maven:3.5 /bin/bash``` 

Let’s create a default maven project

```shell
mvn archetype:generate \
  -DgroupId=app.jgitver.demos \
  -DartifactId=simple-maven-demo \
  -Dversion=1.0.0-SNAPSHOT \
  -Dpackage=app.jgitver.demos \
  -DarchetypeArtifactId=maven-archetype-quickstart \
  -DinteractiveMode=false \
  -DarchetypeCatalog=local
```

Some git

```
cd simple-maven-demo && \
git init && \
git config user.name "User X" && \
git config user.email "userx@nowhere.com" && \
git config --global alias.lg "log --color --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit" && \
echo target/ > .gitignore && \
git add . && \
git commit -m "initial version"
```

Let’s validate `mvn validate` and look at the POM file

```xml
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/maven-v4_0_0.xsd">
  <modelVersion>4.0.0</modelVersion>
  <groupId>app.jgitver.demos</groupId>
  <artifactId>simple-maven-demo</artifactId>
  <packaging>jar</packaging>
  <version>1.0.0-SNAPSHOT</version>
  <name>simple-maven-demo</name>
  <url>http://maven.apache.org</url>
  <dependencies>
    <dependency>
      <groupId>junit</groupId>
      <artifactId>junit</artifactId>
      <version>3.8.1</version>
      <scope>test</scope>
    </dependency>
  </dependencies>
</project>
```

Or simply by using `grep -E --color=auto '^.*1.0.0-SNAPSHOT.*$|$' pom.xml` 

#### jgitver installation

The [jgitver-maven-plugin](http://github.com/jgitver/jgitver) is a core maven extension that needs to be declared inside a file `.mvn/extensions.xml`.

Let's install it.

```sh
sh -c "$(wget -q https://git.io/fA6sj -O -)"
```

So it gives 

```sh
== jgitver-maven-plugin:1.5.1 added under /simple-maven-demo/.mvn/extensions.xml 
```

Let’s verify jgitver working

```
mvn validate
```

```sh
...
[INFO] No suitable configuration file found, using defaults
[INFO] Using jgitver-maven-plugin [1.5.1] (sha1: e45d1669b39cedb98720dd33cc14d0185b455ca1)
[INFO]     version '0.0.0-SNAPSHOT' computed in 450 ms
[INFO]
[INFO] Scanning for projects...
[INFO] jgitver-maven-plugin is about to change project(s) version(s)
[INFO]     fr.brouillard.jgitver.demos::simple-maven-demo::1.0.0-SNAPSHOT -> 0.0.0-SNAPSHOT
[INFO]
[INFO] -----------< app.jgitver.demos:simple-maven-demo >------------
[INFO] Building simple-maven-demo 0.0.0-SNAPSHOT
[INFO] --------------------------------[ jar ]---------------------------------
[INFO] ------------------------------------------------------------------------
[INFO] BUILD SUCCESS
[INFO] ------------------------------------------------------------------------
[INFO] Total time: 0.889 s
[INFO] Finished at: 2019-04-02T11:48:27Z
[INFO] ------------------------------------------------------------------------
```

Version is now overridden by jgitver to `0.0.0-SNAPSHOT` using the history of the project.
`git lg`

Using no special configuration, project versioning starts with `0.0.0` and uses `SNAPSHOT qualifier`.

#### git tagging

In order to be clean, we first need to commit the created maven extension file
`git add . && git commit -m "add jgitver maven extension"`

Our project now has 2 commits `git lg`

```
root@0d9eae28956e:/simple-maven-demo# git lg
* ca6d574 - (HEAD -> master) add jgitver (4 seconds ago) <User X>
* f0bb09a - initial version (12 minutes ago) <User X>
```

Now let's terminate with one of the main interesting feature of [jgitver](http://github.com/jgitver/jgitver): *tagging*:

- create an annotated release tag:
  `git tag -a -m "release 1.0" 1.0`

  `git lg`

```
root@0d9eae28956e:/simple-maven-demo# git lg
* ca6d574 - (HEAD -> master, tag: 1.0) add jgitver (4 seconds ago) <User X>
* f0bb09a - initial version (12 minutes ago) <User X>
```

- now maven project version calculated by jgitver should be `1.0.0`:
  `mvn validate`

jgitver should have hooked in maven build and printed the following

```
...
[INFO] jgitver-maven-plugin is about to change project(s) version(s)
[INFO]     fr.brouillard.jgitver.demos::simple-maven-demo::1.0.0-SNAPSHOT -> 1.0.0
...
```


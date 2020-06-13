# gradle-maven-push
Helper to upload Gradle Java Artifacts to Maven repositories.

Inspired by Chris Banes' [gradle-mvn-push](https://github.com/chrisbanes/gradle-mvn-push).

## Usage

### Update your home gradle.properties

This will include the username and password to upload to the Maven server and so that they are kept local on your machine. The location defaults to `USER_HOME/.gradle/gradle.properties`.

It may also include your signing key id, password, and secret key ring file (for signed uploads).  Signing is only necessary if you're putting release builds of your project on maven central.

```properties
NEXUS_USERNAME=username
NEXUS_PASSWORD=password

signing.keyId=ABCDEF12
signing.password=n1c3try
signing.secretKeyRingFile=~/.gnupg/secring.gpg
```

### Create project root gradle.properties
You may already have this file, in which case just edit the original. This file should contain the POM values which are common to all of your sub-project (if you have any).

```properties
VERSION_NAME=0.1.0-SNAPSHOT
GROUP=io.github.ykayacan

POM_DESCRIPTION=This small library is a Java 11 port of GraphQL DataLoader.
POM_URL=https://github.com/ykayacan/java-dataloader/
POM_SCM_URL=https://github.com/ykayacan/java-dataloader/
POM_SCM_CONNECTION=scm:git:git://github.com/ykayacan/java-dataloader.git
POM_SCM_DEV_CONNECTION=scm:git:ssh://git@github.com/ykayacan/java-dataloader.git

POM_LICENCE_NAME=The Apache Software License, Version 2.0
POM_LICENCE_URL=http://www.apache.org/licenses/LICENSE-2.0.txt
POM_LICENCE_DIST=repo

POM_DEVELOPER_ID=ykayacan
POM_DEVELOPER_NAME=Yasin Sinan Kayacan
```

The `VERSION_NAME` value is important. If it contains the keyword `SNAPSHOT` then the build will upload to the snapshot server, if not then to the release server.

### Create gradle.properties in each sub-project
The values in this file are specific to the sub-project (and override those in the root `gradle.properties`). In this example, this is just the name, artifactId and packaging type:

```properties
POM_NAME=Java DataLoader Library
POM_ARTIFACT_ID=java-dataloader
POM_PACKAGING=jar
```

### Call the script from each sub-modules build.gradle

Add the following at the end of each `build.gradle` that you wish to upload:

```groovy
apply from: 'https://raw.githubusercontent.com/ykayacan/gradle-maven-push/master/gradle-maven-push.gradle'
```

### Build and Push

You can now build and push:

```bash
$ gradle clean build publish
```
	
### Other Properties

There are other properties which can be set:

```
RELEASE_REPOSITORY_URL (defaults to Maven Central's staging server)
SNAPSHOT_REPOSITORY_URL (defaults to Maven Central's snapshot server)
SNAPSHOT_SIGN_ENABLED (defaults to false)
```

## License

```text
Copyright 2020 Yasin Sinan Kayacan

Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at

    http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.
```

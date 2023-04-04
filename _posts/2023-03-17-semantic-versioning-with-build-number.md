---
layout: post
title:  "Gradle Plugin 001 - Semantic versioning with build number"
date:   2023-03-17
excerpt: "Semantic versioning with build number"
image: "/images/gradle.jpg"
---

## Gradle Plugin 001 - Semantic versioning with build number

It's been a minute since I've posted anything, I'm going to try to be a bit better about it moving forward. To start this
year off I've been working on building a deeper understanding of the Gradle build system and one of the ways I'm doing
that is building my own gradle plugins. After years of simply using the system and writing super complex build files I've
decided to try to distill that knowledge into some reusable plugins that I can share with the community. My very first
attempt at doing this is my new `semantic-versioning-with-build-number` plugin.

You may ask "Why build another semantic versioning plugin"? Well, because I tried a number of the ones available and
none of them worked how I wanted them to work, or they might have, but even testing several recent ones I had trouble
getting them to work correctly due to missing documentation.

Plugins are currently available at `plugins.gradle.org`, search for `benrhine` or follow the links below.
- [semantic-versioning-with-build-number](https://plugins.gradle.org/plugin/com.benrhine.semantic-versioning-with-build-number)
- [semantic-versioning-with-build-number-kotlin](https://plugins.gradle.org/plugin/com.benrhine.semantic-versioning-with-build-number-kotlin)

### How did I want it to work?
So how did I want it to work? I wanted the plugin to fully take over setting the `version` variable within any gradle project.
I didn't want version to be declared anywhere else in the build unless it was being referenced. I also wanted the ability
to be able to programmatically operate on the build number using CI scripts. Lastly, and maybe more importantly I wanted
the ability to include a build number in my versioning and that was not a feature I was able to find in any of the other
plugins I surveyed.

### Overview
I am not going to go into full documentation on how it works or how to use it in this post. For those details please go
read the rigorous documentation I have put together [here](https://github.com/benrhine/semantic-versioning-with-build-number/blob/main/README.md).
If you want to include the build number it is assumed you are using a remote CI service of some sort (GitHub Actions, BitBucket, ect ...).
In order for this to work you must configure the `remoteBuild` property discussed below under [Configuration](#configuration) as well
as supply your CI services pre-defined ENV VAR name in `ciBuildNumberEnvVarName`. This suggests the GitHub Actions or the
BitBucket var names by default. If you know of others from other services please reach out to me and I will be happy
to update the plugin and add them.

#### How to include in YOUR project.
If you are using modern gradle ...
```groovy
plugins {
    id 'com.benrhine.semantic-versioning-with-build-number' version '0.0.1'
}
```
If you are using older gradle or attempting to build the plugin from source ...
```groovy
buildscript {
    repositories {
        mavenLocal()
        mavenCentral()
    }
    dependencies {
        classpath 'com.benrhine:semantic-versioning-with-build-number:0.0.1'
    }
}

plugins {
  
}

apply plugin: 'com.benrhine.semantic-versioning-with-build-number'
```
Once the plugin is applied you **MUST** add the following to your `gradle.properties` file, otherwise you will get errors.
```properties
major=0
minor=0
patch=1
buildNumber=0
artifact-type=LOCAL
remote-build=false
include-release-tag=false
```

### Configuration
I wanted this plugin to be super easy to use since I had issues with some of the others I tried, so with that in mind
no configuration is necessary by default. If you applied the plugin as stated above it is now managing your version number.
Now, if the default isn't good enough for your purposes (and it probably isn't) then a number of configuration options 
are available to you.
```groovy
versionConfig {
    remoteBuild = true
    ciBuildNumberEnvVarName = "BUILD_RUN_NUMBER" //BITBUCKET_BUILD_NUMBER
    artifactType = "SNAPSHOT"
    includeReleaseTag = true
    includeBuildNumber = true
    customVersionPropertiesPath = "$projectDir/src/main/resources/version.properties"
}
```
For a full run down on the above options and how they work see the documentation [here](https://github.com/benrhine/semantic-versioning-with-build-number#configuration).

### Available Tasks
So what [tasks](https://github.com/benrhine/semantic-versioning-with-build-number#available-tasks) are available out of the box?
#### Print the current version
```shell
./gradlew printVersion
```

#### Increment the patch portion of the version
```shell
./gradlew incrementPatchVersion
```

#### Increment the minor portion of the version
```shell
./gradlew incrementMinorVersion
```

#### Increment the major portion of the version
```shell
./gradlew incrementMajorVersion
```

#### Decrement the patch portion of the version
```shell
./gradlew decrementPatchVersion
```

#### Decrement the minor portion of the version
```shell
./gradlew decrementMinorVersion
```

#### Decrement the major portion of the version
```shell
./gradlew decrementMajorVersion
```

#### Project References
- [GitHub Repo](https://github.com/benrhine/semantic-versioning-with-build-number)
- [Detailed Documentation](https://github.com/benrhine/semantic-versioning-with-build-number/blob/main/README.md)
- [semantic-versioning-with-build-number](https://plugins.gradle.org/plugin/com.benrhine.semantic-versioning-with-build-number)
- [semantic-versioning-with-build-number-kotlin](https://plugins.gradle.org/plugin/com.benrhine.semantic-versioning-with-build-number-kotlin)

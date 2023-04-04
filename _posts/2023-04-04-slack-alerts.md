---
layout: post
title:  "Gradle Plugin 002 - Slack Alerts"
date:   2023-04-04
excerpt: "Slack Alerts"
image: "/images/gradle.jpg"
---

## Gradle Plugin 002 - Slack Alerts

For my next plugin I wanted to vastly simplify how to send slack alerts from a Gradle build. This is something I have done
extensively over the last few years but have primarily leveraged cURL in order to achieve. While there is nothing wrong
with using cURL to do this I wanted a more idiomatic way of accomplishing this. Similar to my [semantic-versioning-with-build-number plugin](https://benrhine.com/blog/semantic-versioning-with-build-number/)
I was unable to find an existing plugin that functioned the way I wanted it to, so I have created my own `slack-alerts-groovy`
plugin.

So why did I create this plugin? Well I had found some other plugins that did great static alerts but were not capable of 
doing any kind of dynamic alerts. I found this exceptionally odd as the reason you would want to send most alerts from your
build requires that they be dynamic. It's great to send a Slack message that tells you a build started ... it's better to
send one that tells you your tests passed, what the test count is, and your coverage report.

Plugins are currently available at `plugins.gradle.org`, search for `benrhine` or follow the links below.
- [slack-alerts-groovy](https://plugins.gradle.org/plugin/com.benrhine.slack-alerts-groovy)
- I may add a kotlin version of this in the future

**Note: This plugin has been tested with both legacy Slack WebHooks and Slack Bots, and it works with both.**

### Overview
I am not going to go into full documentation on how it works or how to use it in this post. For those details please go
read the rigorous documentation I have put together [here](https://github.com/benrhine/slack-alerts-groovy). This plugin
supports global config of common properties, as many static Slack messages as you wish to configure, and ten predefined
dynamic alerts for test results and application health.

#### How to include in YOUR project.
If you are using modern gradle ...
```groovy
plugins {
    id 'com.benrhine.slack-alerts-groovy' version '0.0.1'
}
```
If you are using older gradle or attempting to build the plugin from source ...
```groovy
buildscript {
    repositories {
        maven {
            url = uri("https://plugins.gradle.org/m2/")
        }
    }
    dependencies {
        classpath("com.benrhine:slack-alerts-groovy:0.0.1")
    }
}

plugins {
  
}

apply plugin: 'com.benrhine.slack-alerts-groovy'
```

### Configuration
Similar to the versioning plugin I wanted the configuration to be super easy. You can apply the plugin with no config and 
your build will not be affected prior to you adding any config. Additionally, you do not need to use the global config
unless you want to. For full details on default configuration see [here](https://github.com/benrhine/slack-alerts-groovy#configuration).
If you wish to use the global config you may add the block below.
```groovy
slackConfig {
    environment = ""        // (Optional) Define what environment an alert is coming from
    webHook = ""            // (Optional) Define the webHook url
    uploadUrl = ""          // (Optional) Define the upload url
    token = ""              // (Optional) Define the auth token for a slack bot
    channels = ""           // (Optional) Define channel or channels
    payload = ""            // (Optional) Additional payload
    displayLogging = ""     // (Optional) Define if logging is enabled
}
```

### Predefined Alerts / Tasks
So what [alerts / tasks](https://github.com/benrhine/slack-alerts-groovy#dynamically-generated-alerts) are available out of the box?
Be aware the following still require you to declare these blocks in your Gradle DSL in order to function. Also, if you
name your DSL block something different the generated alert / task name will be somewhat different, these work off the
core portion of the DSL block name (i.e. unitTest, intTest, ect...) so if you want to name your DSL block `myUnitTestExtreme`
you absolutely can but now your generated task name will be `sendMyUnitTestExtremeAlert`.
#### sendUnitTestAlert
```shell
./gradlew sendUnitTestAlert 
```

#### sendUnitTestResult
```shell
./gradlew sendUnitTestResult
```

#### sendIntTestAlert
```shell
./gradlew sendIntTestAlert
```

#### sendIntTestResult
```shell
./gradlew sendIntTestResult
```

#### sendLoadTestAlert
```shell
./gradlew sendLoadTestAlert
```

#### sendAuthenticatedSmokeTestAlert
```shell
./gradlew sendAuthenticatedSmokeTestAlert
```

#### sendValidationSmokeTestAlert
```shell
./gradlew sendValidationSmokeTestAlert
```

#### sendUnauthenticatedSmokeTestAlert
```shell
./gradlew sendUnauthenticatedSmokeTestAlert
```

#### sendApplicationHealthCheckAlert
```shell
./gradlew sendApplicationHealthCheckAlert
```

#### sendApplicationInfoAlert
```shell
./gradlew sendApplicationInfoAlert
```

### Troubleshooting
Since the alert names are generated this can cause build annoyances from time to time if you change a DSL block name and
the alert / task name is not regenerated. If this happens comment out any usages of the alert, comment out the dsl block,
clean the project, then reload Gradle, uncomment the usages, and reload Gradle again, and it should resolve the issue.

#### Project References
- [GitHub Repo](https://github.com/benrhine/slack-alerts-groovy)
- [Detailed Documentation](https://github.com/benrhine/slack-alerts-groovy/blob/main/README.md)
- [slack-alerts-groovy](https://plugins.gradle.org/plugin/com.benrhine.slack-alerts-groovy)

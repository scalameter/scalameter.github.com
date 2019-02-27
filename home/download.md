---
layout: default
title: Download
permalink: /home/download/index.html
---


## Direct download

The latest ScalaMeter release is **ScalaMeter 0.17** for Scala 2.11, Scala 2.12 and Scala 2.13.

In case this page was not updated, please consider checking the [Maven repositories too]().


## Maven repository

ScalaMeter is available for download from Maven Central!

```
<dependency>
    <groupId>com.storm-enroute</groupId>
    <artifactId>scalameter_2.11</artifactId>
    <version>0.17</version>
    <exclusions>
        <exclusion>
            <groupId>org.mongodb</groupId>
            <artifactId>casbah_2.11</artifactId>
        </exclusion>
    </exclusions>
</dependency>
<dependency>
    <groupId>org.mongodb</groupId>
    <artifactId>casbah_2.11</artifactId>
    <version>3.1.1</version>
    <type>pom</type>
</dependency>
```

If you're using [SBT](/home/gettingstarted/sbt/),
just add the following lines to `build.sbt`:

    resolvers += "Sonatype OSS Snapshots" at
      "https://oss.sonatype.org/content/repositories/releases"
    
    libraryDependencies += "com.storm-enroute" %% "scalameter" % "0.17"

    testFrameworks += new TestFramework("org.scalameter.ScalaMeterFramework")
    
    parallelExecution in Test := false

To make sure that you are using the JLine-based output about the benchmark execution,
add the following to your `build.sbt`:

    fork := true
    
    outputStrategy := Some(StdoutOutput)
    
    connectInput := true

To use `scalameter-core` module for
[lightweight inline benchmarking](/home/gettingstarted/0.7/inline/), add the following:

    resolvers += "Sonatype OSS Snapshots" at
      "https://oss.sonatype.org/content/repositories/releases"

    libraryDependencies +=
      "com.storm-enroute" %% "scalameter-core" % "0.17"


## Source code

ScalaMeter source code is available at
[GitHub](https://github.com/scalameter/scalameter).


## Snapshot version

To access the cutting-edge ScalaMeter snapshots and use the latest ScalaMeter
features from the latest pull request,
enter the following to your `build.sbt`:

    resolvers += "Sonatype OSS Snapshots" at
      "https://oss.sonatype.org/content/repositories/snapshots"
    
    libraryDependencies +=
      "com.storm-enroute" %% "scalameter" % "0.18-SNAPSHOT" % "test"
    
    testFrameworks += new TestFramework("org.scalameter.ScalaMeterFramework")
    
    parallelExecution in Test := false


You can see the current
[build status here](https://travis-ci.org/scalameter/scalameter).

[![Build Status](https://travis-ci.org/scalameter/scalameter.png?branch=master)](https://travis-ci.org/scalameter/scalameter)

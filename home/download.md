---
layout: default
title: Download
permalink: /home/download/index.html
---



## Direct download

The latest ScalaMeter release is **ScalaMeter 0.7** for Scala 2.10 and Scala 2.11.


## Maven repository

ScalaMeter is available for download from Maven Central!

    <dependency>
      <groupId>com.storm-enroute</groupId>
      <artifactId>scalameter_2.11</artifactId>
      <version>0.7</version>
    </dependency>

If you're using [SBT](/home/gettingstarted/sbt/),
just add the following lines to `build.sbt`:

    resolvers += "Sonatype OSS Snapshots" at
      "https://oss.sonatype.org/content/repositories/releases"
    
    libraryDependencies += "com.storm-enroute" %% "scalameter" % "0.7"

    testFrameworks += new TestFramework("org.scalameter.ScalaMeterFramework")
    
    parallelExecution in Test := false

To use `scalameter-core` module for
[lightweight inline benchmarking](/home/gettingstarted/0.7/inline/), add the following:

    resolvers += "Sonatype OSS Snapshots" at
      "https://oss.sonatype.org/content/repositories/releases"

    libraryDependencies +=
      "com.storm-enroute" %% "scalameter-core" % "0.7"


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
      "com.storm-enroute" %% "scalameter" % "0.8-SNAPSHOT" % "test"
    
    testFrameworks += new TestFramework("org.scalameter.ScalaMeterFramework")
    
    parallelExecution in Test := false


You can see the current
[build status here](https://travis-ci.org/scalameter/scalameter).

[![Build Status](https://travis-ci.org/scalameter/scalameter.png?branch=master)](https://travis-ci.org/scalameter/scalameter)


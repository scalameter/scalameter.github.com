---
layout: default
title: Release Process
permalink: /home/releasing/index.html
---



## Library

ScalaMeter release process is automated thanks to [sbt-release](https://github.com/sbt/sbt-release).
It is triggered using `sbt release`.

On each release following steps are performed:
- checking snapshot dependencies and querying user on continue if any snapshot dependency is found
- querying user for release and next snapshot version
- running tests
- setting and commiting release version to master branch
- creating new version branch from master in format `version/$releaseVersion`
- pushing local version branch to remote
- publishing project artifacts to Sonatype
- pushing changes from local version branch to remote
- setting and commiting snapshot version to master branch
- pushing changes from local master branch to remote


## Examples

Invoking release process updates also [scalameter-examples](https://github.com/scalameter/scalameter-examples).

After steps listed in [Library](##Library) section additional work is done, which involves:
- cloning scalameter-examples to temporary directory
- updating version of ScalaMeter artifact to release version in each example respectively
- commiting all changes
- tagging examples repo with tag in format `v$releaseVersion`
- updating version of ScalaMeter artifact to snapshot version in each example respectively
- commiting all changes
- pushing all changes to remote

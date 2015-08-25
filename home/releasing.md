---
layout: default
title: Release Process
permalink: /home/releasing/index.html
---



## Library

ScalaMeter release process is automated thanks to
[sbt-release](https://github.com/sbt/sbt-release).
To cut a new release, follow these steps:

1. Run `sbt release` inside the ScalaMeter repo.

When a release is cut, the following steps are performed:

- checking snapshot dependencies and querying user on continue if any snapshot
  dependency is found
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

Invoking the release process also updates
[scalameter-examples](https://github.com/scalameter/scalameter-examples).

After the steps listed in [Library](#library) section, the following ones are performed:

- cloning scalameter-examples to a temporary directory
- updating the version of the ScalaMeter artifact to the latest release version
  in each example, respectively
- commiting all changes
- tagging examples repo with tag in format `v$releaseVersion`
- updating the version of the ScalaMeter artifact to the snapshot version
  in each example, respectively
- commiting all changes
- pushing all changes to remote

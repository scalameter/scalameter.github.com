---
layout: docsdefault05
title: Inline Benchmarking
permalink: /home/gettingstarted/0.5/inline/index.html

smversion: 0.5
partof: getting-started
num: 9
outof: 9
---


ScalaMeter 0.5 introduces a great new feature -- inline benchmarking.
Instead of writing a performance unit test in the test folder of your project,
you can now use ScalaMeter to introspect performance of parts of your code as you develop your application.

First, make sure you add the following dependency to your project:

    resolvers += "Sonatype OSS Snapshots" at
      "https://oss.sonatype.org/content/repositories/snapshots"
    libraryDependencies += "com.storm-enroute" %% "scalameter-core" % "[version]"

where `[version]` is at least `0.5-SNAPSHOT`, and you are using Scala 2.10 or newer.
Note that the minimalistic `scalameter-core` module is sufficient for inline benchmarking and you do not need to use the full ScalaMeter suite.

Then, to measure how long a snippet of code executes, proceed as follows:

    import org.scalameter._
    val time = measure {
      for (i <- 0 until 100000) yield i
    }
    println(s"Total time: $time")

By default, ScalaMeter executes inline benchmarks only once and without any warming.
It measures the running time of the snippet.
However, you can change the configuration of your microbenchmark as if it were a performance unit test.
In the following, we turn on verbose logging, set the number of repetitions to 20, turn on the default warmer
and use the measurer that ignores GC cycles.

    val time = config(
      Key.exec.benchRuns -> 20,
      Key.verbose -> true
    ) withWarmer {
      new Warmer.Default
    } withMeasurer {
      new Measurer.IgnoringGC
    } measure {
      for (i <- 0 until 100000) yield i
    }
    println(s"Total time: $time")

To measure a different metric, such as memory footprint, instantiate a different measurer:

    val mem = config(
      Key.exec.benchRuns -> 20
    ) withMeasurer(new Measurer.MemoryFootprint) measure {
      for (i <- 0 until 100000) yield i
    }
    println(s"Total memory: $mem")

Note that all the inline benchmarks are executed in the same JVM -- inline benchmarks act as if they were run in a `LocalExecutor`.

More examples [here](https://github.com/scalameter/scalameter/blob/master/src/test/scala/org/scalameter/inline/InlineBenchmarkTest.scala).


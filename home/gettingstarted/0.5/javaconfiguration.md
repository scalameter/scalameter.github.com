---
layout: default
title: Test configuration for java users
permalink: /configurationforjava/index.html

smversion: 0.5
partof: getting-started
num: 3
outof: 9
---


In this section we will see how to configure how performance tests are executed,
how results are reported as well as tweak a range of test parameters
selectively.
Before we start, we note that we will be running all the tests from within
an SBT project from now on.
This might be a good idea to read up on [SBT integration](/home/gettingstarted/sbt/)
if you haven't already.


## Predefined configurations

These abstract classes predefined some configurations (they do the same thing as those defined for scala benchmarks):
<br/>
-`QuickBenchmark` -- Good for quick testing to get a feel for how long something takes.
<br/>
-`MicroBenchmark`-- Runs a separate JVM and logs outputs to console, but generates no HTML document and does no regression testing. Good for reliable and repeatable testing where micro-optimizations play a role.
<br/>
- `OnlineRegressionReport` -- Runs in a separate JVM, generates a HTML report ready to be hosted online, and does regression testing. Method of choice for continuous integration.
<br/>
- `OfflineRegressionReport` -- Runs in a separate JVM, generates a HTML report ready to be viewed in your browser from `file://` schema, and does regression testing. Ideal for local development on existing codebase when source code changes must not cause performance regressions.
<br/>
- `OfflineReport` -- Runs in a separate JVM, generates a HTML report for offline viewing in the browser, and does no regression testing. Ideal when developing a new codebase or tweaking the code to understand performance impacts.
<br/>

You can extend these abstract classes to have these configurations.


## configurations

You can also do your own configuration if you define an `executor`, a `reporter` and a `persistor`.
Here is an example with a modified version of what you can find in the <a href="/home/gettingstarted/simplemicrobenchmarkforjava/index.html">Simple benchmark for java users</a> section.


    import org.scalameter.JavaPerformanceTest;
    import org.scalameter.javaApi.*;
    import java.util.LinkedList;

    public class ListBenchmark
    extends JavaPerformanceTest {
    public Executor javaExecutor(){
        Executor exec = new LocalExecutor(new Warmer().Default(),
            new MinAggregator(),
            new DefaultMeasurer());
        return exec;
    }
    public Measurer javaMeasurer(){
        return new DefaultMeasurer();
    }
    public Reporter javaReporter(){
        return new LoggingReporter();
    }
    public Persistor javaPersistor(){
        return new NonePersistor();
    }
        public class ListBench implements 
        org.scalameter.javaApi.Using<LinkedList<Integer>, LinkedList<Integer>> {
            public JavaGenerator<LinkedList<Integer>> generator(){
                JavaGenerator<Integer> range = new RangeGen("Range", 1, 21, 4);
                    return new Collections(range).lists();
            }
            
            public LinkedList<Integer> snippet(LinkedList<Integer> in){
                LinkedList<Integer> ret = new LinkedList<Integer>();
                for(int i = 0; i < in.size(); i++){
                    Integer a = in.get(i);
                    ret.add(a+1);
                }
                return ret;
            }
        }
    }

We just defined here the configuration exactly how QuickBenchmark is defined. There are other possible configurations available in `org.scalameter.javaApi` so you can choose the `executor`, `measurer`, `reporter`, and `persistor` to use.
<br/>
Here is the list of available configurations (a complete description of what they do can be found in the <a href="/home/gettingstarted/0.5/executors/index.html">executors</a> for scala benchmarks section):
<br/>
The `Executor`s:
<br/>
- `LocalExecutor` -- Executes tests in the same JVM from which ScalaMeter was invoked.
<br/>
- `SeparateJvmsExecutor` -- Executes tests in a different JVM.
<br/>
The `Measurer`s:
<br/>
- `DefaultMeasurer` -- Does as many running time measurements as the user requests.
<br/>
- `TimeWithIgnoringGC` -- Does the same as `DefaultMeasurer`, but ignores those measurements with a GC cycle.
<br/>
- `TimeWithIgnoringGCWithOutlierElimination` -- It corresponds to `Measurer.IgnoringGC` with `Measurer.OuterElimination`. `Measurer.OuterElimination` analyzes the measurements returned by other measurers and possibly discards and repeats measurements.
<br/>
- `TimeWithIngoringGCWithPeriodicReinstantiationWithOutlierElimination` -- Same as above but also with `PeriodicReinstantiation`.
<br/>
- `MemoryFootprint` -- Used to measure the MemoryFootprint.
<br/>
The `Persistor`s (for more informations about persistors see <a href="/home/gettingstarted/0.5/regressions/index.html">regressions</a> section):
<br/>
- `NonePersistor` -- Does not save the results.
<br/>
- `SerializationPersistor` -- Serialize the results and store them.
<br/>
The `Reporter`:
<br/>
- `LoggingReporter` -- Outputs the results of the tests into the terminal.


## Configuring parameters
For more configuration (such as number of benchRuns, number of warmups, ...) you can define your using block in a class implementing the `Group` interface and define the `config()` method.

    import org.scalameter.QuickBenchmark;
    import org.scalameter.javaApi.*;
    import java.util.LinkedList;
    import java.util.HashMap;

    public class ListBenchmark
    extends QuickBenchmark {
        public class ListTestGroup implements Group{
            public HashMap<exec, Object> config(){
                HashMap<exec, Object> config = new HashMap<exec, Object>();
                config.put(exec.benchRuns, 20);
                config.put(exec.minWarmupRuns, 10);
                config.put(exec.maxWarmupRuns, 15);
                return config;
            }
            public class ListBench implements 
            org.scalameter.javaApi.Using<LinkedList<Integer>, LinkedList<Integer>> {
                public JavaGenerator<LinkedList<Integer>> generator(){
                    JavaGenerator<Integer> range = new RangeGen("Range", 1, 21, 4);
                        return new Collections(range).lists();
                }
            
                public LinkedList<Integer> snippet(LinkedList<Integer> in){
                    LinkedList<Integer> ret = new LinkedList<Integer>();
                    for(int i = 0; i < in.size(); i++){
                        Integer a = in.get(i);
                        ret.add(a+1);
                    }
                    return ret;
                }
            }
        }
    }


Here we configured the test to run `20` times, with at least `10` warmup runs and at most `15`. The `Group` interface is used to nest test scopes.
The configurations of a `Group` will be used in every inner test class implementing the `Using` interface. You can also define the `config()` method inside a `Using` block.
For every test, the innermost configuration is taken.





















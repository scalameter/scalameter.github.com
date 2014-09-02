---
layout: docsdefault05
title: Simple benchmark
permalink: /home/gettingstarted/0.7/javausage/index.html

smversion: 0.7

partof: getting-started
num: 10
outof: 10
---

ScalaMeter tests can be (despite what the name might suggest) written in Java as well.
This section shows the basics of how to do this.

Lets create a new file named `JavaRegressionTest2.java`.
We will use it to write an offline report test, which tests the performance of a histogram application.
We will traverse a linked list and group its elements to different linked lists in a hash map.
We start by importing the following package, which allows us to use the ScalaMeter's Java interface:

    import org.scalameter.japi.*;

Next, we import the following classes for our test:

    import java.util.HashMap;
    import java.util.Iterator;
    import java.util.LinkedList;

We extend the `OfflineReport` class -- you can read more about it in the [configuration section](/home/gettingstarted/0.7/configuration/index.html):

    public class MyHistogramTest extends OfflineReport {

Next, we implement the `javaPersistor` method -- you can read more about this in the [configuration section](/home/gettingstarted/0.7/configuration/index.html):

      public Persistor javaPersistor() {
        return new SerializationPersistor();
      }

Benchmarks in a test are organized into groups.
In the Scala frontend, we encoded the groups using nested closures.
In the Java frontend, we encode them by implementing the `Group` interface.
We now declare a `List` benchmark group:

      public class List implements Group {

Next, we declare a `Using` class for the benchmark.
In the Scala frontend, this is achieved with the `using` block, as described in the [simple microbenchmark example](/home/gettingstarted/0.7/simplemicrobenchmark/index.html).

        public class groupBy implements org.scalameter.japi.Using<LinkedList<Integer>, HashMap<Integer, LinkedList<Integer>>> {

To override the default configuration for a `Using` class or a `Group`,
we declare a final `config` field of the type `JContext` (more about custom per-group configurations [here](/home/gettingstarted/0.7/configuration/index.html)):

          public final JContext config = JContext.create()
            .put("exec.benchRuns", 20)
            .put("exec.independentSamples", 1)
            .put("exec.outliers.covMultiplier", 1.5)
            .put("exec.outliers.suspectPercent", 40);

Next, we declare a generator for the `groupBy` class (see more about generators in the [generator section](/home/gettingstarted/0.7/generators/index.html)):

          public JavaGenerator<LinkedList<Integer>> generator() {
            JavaGenerator<Integer> sizes = new SingleGen("size", 5000000);
            return new CollectionGenerators(sizes).lists();
          }

Finally, we declare the code snippet for the test by implementing the `snippet` method:

          public HashMap<Integer, LinkedList<Integer>> snippet(LinkedList<Integer> in) {
            HashMap<Integer, LinkedList<Integer>> hm = new HashMap<Integer, LinkedList<Integer>>();
            for (int i = 0; i < 10; i++) {
              hm.put(i, new LinkedList<Integer>());
            }
            Iterator<Integer> it = in.iterator();
            while (it.hasNext()) {
              Integer element = it.next();
              LinkedList<Integer> tmp = hm.get(element % 10);
              tmp.add(element);
              hm.put(element % 10, tmp);
            }
            return hm;
          }
        }
      }
    }

The complete test is here:

    public class MyHistogramTest extends OfflineReport {
      public Persistor javaPersistor() {
        return new SerializationPersistor();
      }
      public class List implements Group {
        public class groupBy implements org.scalameter.japi.Using<LinkedList<Integer>, HashMap<Integer, LinkedList<Integer>>> {
          public final JContext config = JContext.create()
            .put("exec.benchRuns", 20)
            .put("exec.independentSamples", 1)
            .put("exec.outliers.covMultiplier", 1.5)
            .put("exec.outliers.suspectPercent", 40);
          public JavaGenerator<LinkedList<Integer>> generator() {
            JavaGenerator<Integer> sizes = new SingleGen("size", 5000000);
            return new CollectionGenerators(sizes).lists();
          }
          public HashMap<Integer, LinkedList<Integer>> snippet(LinkedList<Integer> in) {
            HashMap<Integer, LinkedList<Integer>> hm = new HashMap<Integer, LinkedList<Integer>>();
            for (int i = 0; i < 10; i++) {
              hm.put(i, new LinkedList<Integer>());
            }
            Iterator<Integer> it = in.iterator();
            while (it.hasNext()) {
              Integer element = it.next();
              LinkedList<Integer> tmp = hm.get(element % 10);
              tmp.add(element);
              hm.put(element % 10, tmp);
            }
            return hm;
          }
        }
      }
    }


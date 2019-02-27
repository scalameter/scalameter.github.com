---
layout: docsdefault07
title: Java Usage
permalink: /home/gettingstarted/0.7/javausage/index.html

smversion: 0.7
partof: getting-started
num: 11
outof: 50
---

ScalaMeter tests can be (despite what the name might suggest) written in Java as well.
This section shows the basics of how to do this.
The short story is: Java users use an alternative annotation-based frontend,
where generators, setups, teardowns, configs and benchmarks snippets are specified
with annotations instead of a special DSL.
This frontend is available for both Java and Scala users.

Lets create a new file named `MyHistogramTest.java`.
We will use it to write an offline report test,
which tests the performance of a histogram application.
We will traverse a linked list and
group its elements to different linked lists in a hash map.
We start by importing the following package,
which allows us to use the ScalaMeter's Java interface:

    import org.scalameter.japi.*;
    
Next, we import required anntoations from the `annotation` subpackage:

    import org.scalameter.japi.annotation.*;

We should also import the following classes for our test:

    import java.util.HashMap;
    import java.util.Iterator;
    import java.util.LinkedList;

We extend the `JBench.OfflineReport` class --
you can read more about it in the
[configuration section](/home/gettingstarted/0.7/configuration/index.html):

    public class MyHistogramTest extends JBench.OfflineReport {

Benchmarks in a test are organized into groups.
In the Scala frontend, we encode the groups using nested closures.
In the Java frontend, we encode them by placing the `benchmark` annotation 
whose `String` argument denotes the group that the benchmark belongs to.
We now declare benchmark snippet stub in the `ops` group that is nested
in the `list` group:

      @benchmark("list.ops")
      public void histogram() {
      
      }
      
Next, we want to declare a custom configuration context, which we will name `config`:

      public final Context config = new ContextBuilder()
          .put("exec.benchRuns", 20)
          .put("exec.independentSamples", 1)
          .put("exec.outliers.covMultiplier", 1.5)
          .put("exec.outliers.suspectPercent", 40)
          .build();
          
Let's assume that we want to apply this custom `config` not only to our
`histogram` method, but to all benchmark snippets in the `list` group.
We do this by adding the optional `scopes` annotation to the `MyHistogramTest` class,
as follows:
    
    @scopes({
      @scopeCtx(scope = "list", context = "config")
    })
    public class MyHistogramTest extends JBench.OfflineReport {

Next, we declare a generator for our snippet
(see more about generators in the
[generator section](/home/gettingstarted/0.7/generators/index.html)).

      public final JGen<Integer> sizes = JGen.intValues("size", 500000);
    
      public final JGen<Tuple2<Integer, LinkedList<Integer>>> lists = 
        sizes.zip(sizes.map(
            new Fun1<Integer, LinkedList<Integer>>() {
              public LinkedList<Integer> apply(Integer v) {
                return new LinkedList<Integer>();
              }
            }
        ));
      
      @gen("lists")
      @benchmark("list.ops")
      public void histogram(Tuple2<Integer, LinkedList<Integer>> v) {
        
      }

Now, assume that the list needs to be initialized with random data before the
benchmark begins.
We will declare our own custom setup method named `listSetup`
that does this initialization.
Then, to ensure that `listSetup` is invoked before the benchmark,
we need to add the `@setup` annotation to the `histogram` method:

      public void listSetup(Tuple2<Integer, LinkedList<Integer>> v) {
        int size = v._1();
        LinkedList<Integer> list = v._2();
    
        Random random = new Random(size);
        for (int i = 0; i < size; i++) {
          list.add(random.nextInt(size));
        }
      }
      
      @gen("lists")
      @setup("listSetup")
      @benchmark("list.ops")
      public void histogram(Tuple2<Integer, LinkedList<Integer>> v) {
        
      }
      
By default, the curve corresponding to the benchmark snippet
will be equal to the method name.
If we would like a custom name for this method, we need to use the `@curve` annotation:

      @gen("lists")
      @setup("listSetup")
      @curve("groupBy")
      @benchmark("list.ops")
      public void histogram(Tuple2<Integer, LinkedList<Integer>> v) {
        
      }

Finally, we add the actual code of the snippet method:

    @gen("lists")
    @setup("listSetup")
    @curve("groupBy")
    @benchmark("list.ops")
    public HashMap<Integer, LinkedList<Integer>> histogram(
      Tuple2<Integer, LinkedList<Integer>> v
    ) {
      LinkedList<Integer> list = v._2();
  
      HashMap<Integer, LinkedList<Integer>> hm =
          new HashMap<Integer, LinkedList<Integer>>();
      for (int i = 0; i < 10; i++) {
        hm.put(i, new LinkedList<Integer>());
      }
      Iterator<Integer> it = list.iterator();
      while (it.hasNext()) {
        Integer element = it.next();
        LinkedList<Integer> tmp = hm.get(element % 10);
        tmp.add(element);
        hm.put(element % 10, tmp);
      }
      return hm;
    }

The complete test is here:
  
    @scopes({
      @scopeCtx(scope = "list", context = "config")
    })
    public class MyHistogramTest extends JBench.OfflineReport {
      public final Context config = new ContextBuilder()
          .put("exec.benchRuns", 20)
          .put("exec.independentSamples", 1)
          .put("exec.outliers.covMultiplier", 1.5)
          .put("exec.outliers.suspectPercent", 40)
          .build();
    
      public final JGen<Integer> sizes = JGen.intValues("size", 500000);
    
      public final JGen<Tuple2<Integer, LinkedList<Integer>>> lists = 
        sizes.zip(sizes.map(
            new Fun1<Integer, LinkedList<Integer>>() {
              public LinkedList<Integer> apply(Integer v) {
                return new LinkedList<Integer>();
              }
            }
        ));
    
      public void listSetup(Tuple2<Integer, LinkedList<Integer>> v) {
        int size = v._1();
        LinkedList<Integer> list = v._2();
    
        Random random = new Random(size);
        for (int i = 0; i < size; i++) {
          list.add(random.nextInt(size));
        }
      }
    
      @gen("lists")
      @setup("listSetup")
      @curve("groupBy")
      @benchmark("list.ops")
      public HashMap<Integer, LinkedList<Integer>> histogram(
        Tuple2<Integer, LinkedList<Integer>> v
      ) {
        LinkedList<Integer> list = v._2();
    
        HashMap<Integer, LinkedList<Integer>> hm =
            new HashMap<Integer, LinkedList<Integer>>();
        for (int i = 0; i < 10; i++) {
          hm.put(i, new LinkedList<Integer>());
        }
        Iterator<Integer> it = list.iterator();
        while (it.hasNext()) {
          Integer element = it.next();
          LinkedList<Integer> tmp = hm.get(element % 10);
          tmp.add(element);
          hm.put(element % 10, tmp);
        }
        return hm;
      }
    }

A complete example of an SBT project using ScalaMeter from Java is available in the
[ScalaMeter examples repo](https://github.com/scalameter/scalameter-examples/blob/master/jbench/src/bench/java/org/scalameter/examples/ListBenchmark.java).

<div class="imagenoframe">
  <img src="/resources/images/logo-yellow-small.png"/>
</div>

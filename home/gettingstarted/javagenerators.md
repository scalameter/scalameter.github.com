---
layout: docsdefault05
title: Generators for java users
permalink: /generatorsforjava/index.html

smversion: 0.5
partof: getting-started
num: 4
outof: 9
---


The scalameter JavaApi offers some usefull classes and methods to easily create generators.
As for scala performance tests, there are predefined *basic* generators.

`VoidGen(String axisName)`
<br/>
Used for the same purpose as `Gen.unit(axis: String)` in Scala tests. It give in argument to the snippet() an object of value `null`.
This generator is useful when we don't need a range of different inputs,
or there is just one meaningful input that is encoded in the microbenchmark.
For example, measuring the time needed to ping some fixed web address fits
into this category.

`SingleGen<T>(String axisName, T v)`
<br/>
Generates a single, specified value `v` of type `T`

`RangeGen(String axisName, int from, int upto, int hop)`
<br/>
Generates an inclusive range of integer values.
Can be used to create an instance of `Collections`, which gives more types of generator.

`EnumerationGen<T>(String axis, T[] xs)`
<br/>
Generates the enumerated values of type `T`.
Useful when parametrizing benchmarked algorithms or methods with
non-numeric data.

`ExponentialGen(String axis, int from, int until, int factor)`
<br/>

Generates an inclusive exponential range of integer values.
The starting value is `from`, and each subsequent value is mutliplied
by `factor`, until the value `until` is reached.

From these generators, you can create new generators using methods `map(MapFunction<T, S> m)` which takes in argument an object which implemented the interface `MapFunction<T, S>` and defined the method that will be used to map the elements of the generator. The `map` method returns a new `JavaGenerator<S>` object.
Here is an example of how to use it:

    public JavaGenerator<String> generator() {
        JavaGenerator<Integer> sizes = new RangeGen("size", 
                                                1000000, 5000000, 2000000);
        JavaGenerator<String> s = sizes.map(new MapFunction<Integer, String>(){
            public String map(Integer t) {
                return t.toString();
            }
            });
            return s;
        }


The `flatmap(FlatmapFunction<T, S> fm)` is used to do a flatmap on elements of a generator. Used like the map function but `FlatmapFunction<T, S>` has to implement function `public JavaGenerator<S> flatmap(T t)`.
The `zip(JavaGenerator<S>)` returns a new `JavaGenerator<scala.Tuple2<T, S>>` which is the current generator zipped with the one given in argument. This new generator generates objects of type Tuple2 which contains 2 values, of type `T` and `S` accessible using `._1` and `._2`.

An other usefull class for generators is the class `Collections(JavaGenerator<Integer> sizes)`, which has methods such as `lists()` returning a generator of `LinkedList<Integer>` or `arrays()` that creates a generator of `int[]`. The returned generators are created using the `JavaGenerator<Integer> sizes` given at construction.
Here is the list of methods in `Collections` with the type of data the returned `JavaGenerator` generates.

- `lists` -- `LinkedList<Integer>`
- `arrays` -- `int[]`
- `vectors` -- `Vector<Integer>`
- `hashtablemaps` -- `HashMap<Integer, Integer>`
- `linkedhashtablemaps` -- `LinkedHashMap<Integer, Integer>`
- `treemap` -- `TreeMap<Integer, Integer>`
- `hashtablesets` -- `HashSet<Integer>`
- `linkedhashtablesets` -- `LinkedHashSet<Integer>`
- `avlsets` -- `TreeSet<Integer>`


















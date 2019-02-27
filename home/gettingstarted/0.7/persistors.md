---
layout: docsdefault07
title: Persistors
permalink: /home/gettingstarted/0.7/persistors/index.html

smversion: 0.7
partof: getting-started
num: 6
outof: 50
---


`Persistor`s are used to store results from executed benchmarks.

They can be specified by overriding the `persistor` method in your benchmark class.


### Persistor.None

It actually does no persisting at all.
Use this persistor when you don't need to store benchmark result history.


### SerializationPersistor

It uses `Java serialization` under the hood.  

Note that this persistor is not a good choice
if you want to keep your results while upgrading `ScalaMeter` version.


### JSONSerializationPersistor

It writes regression data to disk as JSON. 
It's the fastest among `Persistor`s, although it produces the largest output files.   


### GZIPJSONSerializationPersistor

Default choice for all `HTMLReport` descendants. 

It works like `JSONSerializationPersistor`,
but instead of storing a plain JSON on disk it gzips it first.
This allows to be much more space efficient than `JSONSerializationPersistor` 
whilst being a little slower (but still faster than `SerializationPersistor`).


## Picklers

For JSON-based persistors, `Parameters` are stored as Base64-encoded strings. 
These Base64-encoded strings can be produced by using `Pickler` classes.
For this reason, every `Parameter` (and thus `Gen` implementation) requires a `Pickler`. 

We provide `Pickler`s for most commonly used types, 
which are available with the following import:

    org.scalameter.picklers.Implicits._

If you would like to make own generator or e.g. invoke `Gen.enumeration`
with a custom class, 
then you must write `Pickler` for that class.

If you don't want to implement a `Pickler` for your custom class,
then you can import `org.scalameter.picklers.noPickler._`.
You will not have to implement a custom `Pickler`,
but you will only be able to use the `SerializationPersistor`.

A `Pickler` has to have a no-argument constructor, or be a top-level object.

<div class="imagenoframe">
  <img src="/resources/images/logo-yellow-small.png"/>
</div>

---
title: ScalaMeter Planning
permalink: /changelog/index.html
---

## [0.17] - 2019-02-27

- Add JLine support and a much nicer progress and output formatter.
- Clean up benchmark templates and make them more consistent.
- Add `@disabled` annotation.
- Various minor improvements.

## [0.7] - 2015-08-26

### Added

- Add standalone sbt examples to `scalameter/scalameter-examples`.
- Add binary compatible JSON based persistors: `JSONSerializationPersistor` and
  `GZIPJSONSerializationPersistor`.
- Add `Meausrer.BoxingCount` that can measure (auto)boxing of Scala primitives.
- Add `Measurer.MethodInvocationCount` that can count invocation of arbitrary methods.
- Add `JBench` and entire new annotation based Java API.
- Add `include(new BenchTrait {})` statement used to include benchmark templates that
  are themselves not standalone tests, but can be used only with `include`.
  These new `include` statements can only be invoked from `Bench.Group` benchmark type.
  See example
  [here](https://github.com/scalameter/scalameter-examples/tree/master/include-statements).
  For rationale, see mailing list
  [discussion](https://groups.google.com/forum/#!topic/scalameter/D3bf57PEhDo).


### Changed

- Make `GZIPJSONSerializationPersistor` default for regression testing.
- Rename `PerformanceTest` to `Bench`.
- The `Bench` class (formerly `PerformanceTest`) now takes a type parameter that
  describes the type of the results it produces. For most benchmark types, this is
  just `Double`, but some benchmark types measure running time profiles or method
  invocation counts and have different results. The `Reporter` and `Measurer` must
  have the same type as the benchmark itself.
- Change default directory for the benchamark results to `target/benchmarks`.


### Deprecated

- Move and mark as deprecated `JavaPerformanceTest` and its components to
  `org.scalameter.deprecatedjapi`.
- Add compatibility type alias `PerformanceTest` for `org.scalameter.Bench`
  in the `api` package.
- Deprecate `include[BenchClass]` statement in favor of `include(new BenchTrait {})`
  (the `BenchTrait` is a normal benchmark, but must be implemented in a Scala trait).


### Fixed

- Fix lack of default `reportDir` when running benchmarks using `main` method.
- Improve [release process](http://scalameter.github.io/home/releasing/).

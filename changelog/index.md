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
  See [discussion](https://groups.google.com/forum/#!topic/scalameter/D3bf57PEhDo).

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
- Deprecated `include[BenchClass]` statement 

### Fixed

- Fix lack of default `reportDir` when running benchmarks using `main` method.
- Improve [release process](http://scalameter.github.io/home/releasing/).
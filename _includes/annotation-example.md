
```
class MapBenchmarks extends JBench.ForkedPreciseTime {
  @gen("hashtries")
  @benchmark("maps.apply")
  @curve("hash-trie")
  def hashTrieApply(xs: HashMap[Int, Int]): Long = {
    var i = 0, sum = 0L
    while (i < xs.size) {
      sum += xs.apply(i)
      i += 1
    }
    sum
  }
}
```

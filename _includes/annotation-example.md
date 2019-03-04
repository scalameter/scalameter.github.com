
```
class MapBenchmarks extends JBench.ForkedPreciseTime {
  @gen("hashtries")
  @benchmark("maps.apply")
  @curve("hash-trie")
  def hashTrieApply(input: (Int, HashMap[Integer, Integer])): Long = {
    val (size, trie) = input
    var i = 0, sum = 0L
    while (i < size) {
      sum += trie.apply(i)
      i += 1
    }
    sum
  }
}
```

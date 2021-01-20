```cpp
/**
 * Splits an array based on a delimiter.
 * Ex: with delimiter 0, 
 * [0 4 -5 6 0 1 7 -2 0 0 1 0 7] -> [[4 -5 6], [1 7 -2], [1]] 
 * [5] -> [[5]]
 * [0] -> []
 */ 
template<typename T>
vector<vector<T>> split(vector<T> const& a, T delimiter) {
    vi positions;
    for(int i = 0; i < SZ(a); ++i) {
        if(a[i] == delimiter) {
            positions.pb(i);
        }
    }
    positions.pb(SZ(a));
    vector<vector<T>> result;
    int last_pos = -1;
    for(int pos: positions) {
        int start = last_pos + 1;
        int end = pos - 1;
        if(start <= end) {
            result.pb(vector<T>(a.begin() + start, a.begin() + end + 1));
        }
        last_pos = pos;
    }
    return result;
}
```

```java
List<List<Integer>> splitArray(List<Integer> a, Integer delimiter) {
    List<List<Integer>> result = new ArrayList<>();
    List<Integer> curIntegers = new ArrayList<>();
    for (Integer x : a) {
      if(x.equals(delimiter)) {
        result.add(curIntegers);
        curIntegers = new ArrayList<>();
      } else {
        curIntegers.add(x);
      }
    }
    result.add(curIntegers);
    return result;
}
```
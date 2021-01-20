# Logic
Split the array around the zeros. Solve for each part. 
Max product is the whole array if the number of negative numbers are even.
If odd, remove the numbers till the first negative number or remove the numbers after the last negative number.
Handle special cases (some listed below).
```
0 4 -5 6 0 1 70 -2 0 0 1 0 7 -999999
-5 -2 2 -30 -999999
-8 -999999
0 -999999
1 0 -999999
```
# Java
```java
import java.math.BigInteger;
import java.util.ArrayList;
import java.util.Arrays;
import java.util.List;
import java.util.Scanner;
import java.util.stream.Collectors;

public class Main {
  static Scanner sc = new Scanner(System.in);

  public Main() throws Exception {
    // sc = new Scanner(new File("input.txt"));
    // System.setOut(new PrintStream(new File("output.txt")));
  }

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

  void solve() {
    while (sc.hasNext()) {
      String[] line = sc.nextLine().split("-999999");
      String[] tokens = line[0].split(" ");
      List<Integer> numbers = Arrays.stream(tokens).map(s -> Integer.parseInt(s)).collect(Collectors.toList());      
      List<List<Integer>> parts = splitArray(numbers, 0);

      List<BigInteger> results = new ArrayList<>();
      for(List<Integer> part: parts) {
        if(!part.isEmpty())
          results.add(getMaxRangeProduct(part));
      }

      BigInteger ans = BigInteger.valueOf(Integer.MIN_VALUE);
      if(parts.size() > 1) {
        ans = ans.max(BigInteger.ZERO);
      }
      for (BigInteger x : results) {
        ans = ans.max(x);
      }
      System.out.println(ans);
    }
  }

  BigInteger multiplyAll(List<Integer> a) {
    BigInteger result = BigInteger.ONE;
    for (Integer x : a) {
      result = result.multiply(BigInteger.valueOf(x));
    }
    return result;
  }

  BigInteger getMaxRangeProduct(List<Integer> part) {
    BigInteger result = multiplyAll(part);
    if(result.signum() == -1) {
      int firstNegIndex = 0, lastNegIndex = 0; 
      boolean firstSet = false;
      for (int i = 0; i < part.size(); i++) {
        int x = part.get(i);
        if(x < 0) {
          if(!firstSet) {
            firstNegIndex = i;
            firstSet = true;
          } 
          lastNegIndex = i;
        }
      }
      
      List<Integer> prefix = part.subList(0, firstNegIndex + 1);
      BigInteger startingProduct = multiplyAll(prefix);
      List<Integer> suffix = part.subList(lastNegIndex, part.size());
      BigInteger endingProduct = multiplyAll(suffix);
      
      if(prefix.size() < part.size() && suffix.size() < part.size()) {
        result = result.divide(startingProduct.max(endingProduct));
      } else if(prefix.size() < part.size()) {
        result = result.divide(startingProduct);
      } else if(suffix.size() < part.size()) {
        result = result.divide(endingProduct);
      }
    }
    return result;
  }

  public static void main(String[] args) throws Exception {
    new Main().solve();
  }
}
```
# CPP
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

void solve() {
    string line;
    while (getline(cin, line)) {
        istringstream line_stream(line);
        vi a;
        while(true) {
            int x; line_stream >> x; 
            if(x == -999999) break;
            a.pb(x);
        }
        wi(a)
        wi(split(a, 0LL))
    }
}
```
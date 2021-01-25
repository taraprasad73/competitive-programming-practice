# O(n^2) - just value
```cpp
/**
 * lnds(n) = length of longest non-decreasing subsequence ending at position n
 * a: 0-indexed
 * memo: same size as a, initialized to -1
 */  
ll lnds(int n, vi &a, vi &memo) {
    if(n == 0) return 1;
    if(memo[n] != -1) return memo[n];
    memo[n] = 1;
    for(int i = 0; i < n; ++i) {
        if(a[i] <= a[n]) 
            memo[n] = max(memo[n], lis(i, a, memo) + 1);
    }
    return memo[n];
}
```

# O(n*log(k)) - just value
```cpp
/**
 * a: 0-indexed
 */  
int lnds(vi const& a) {
    vi lis;
    for(int i = 0; i < SZ(a); ++i) {
        auto itr = upper_bound(lis.begin(), lis.end(), a[i]);
        if(itr == lis.end()) 
            lis.pb(a[i]);
        else 
            *itr = a[i];
    }
    return lis.size();
}
```
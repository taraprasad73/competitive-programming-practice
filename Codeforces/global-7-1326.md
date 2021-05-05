# Contest: [Codeforces Global Round 7](https://codeforces.com/contest/1326)
Solutions at https://codeforces.com/contest/1326/my

## [A. Bad Ugly Numbers](https://codeforces.com/contest/1326/problem/A)
Construction logic
```
n = 2: 23
n = 3: 223
n = 4: 2243
n = 5: 22223
n = 6: 222223
n = 7: 2222243
```
Basically, whenever the number of 2s are a multiple of 3 replace one 2 with 4.
```cpp
int t; cin >> t;
while(t--) {
    int n; cin >> n;
    if(n == 1) {
        cout << -1 << endl;    
    } else {
        int twos = n - 1;
        bool four = false;
        if((n - 1) % 3 == 0) {
            twos--;
            four = true;
        }
        for(int i = 0; i < twos; ++i) {
            cout << 2;
        }
        if(four) 
            cout << 4;
        cout << 3 << endl;
    }
} 
```

## [B. Maximums](https://codeforces.com/contest/1326/problem/B)
Since we know what a[0] is, we can calculate the values of a[i] from b[i] at each iteration.
```cpp
int n; cin >> n;
vi a(n), b(n);
ll max_so_far = 0;
for(int i = 0; i < n; ++i) {
    cin >> b[i];
    a[i] = b[i];
    if(i > 0)
        a[i] += max_so_far;
    max_so_far = max(max_so_far, a[i]);
}
for(int i = 0; i < n; ++i) {
    cout << a[i] << " ";
}
```

## [C. Permutation Partitions](https://codeforces.com/contest/1326/problem/C)
The idea was to find the largest k numbers (it can be shown that the _partition value_ is always the sum of the largest k numbers), and then solve a combinatorics problem. The answer to the second part is obtained by calculating the number of possibilities of putting a stick in between the positions of the largest k numbers, so as to separate the largest k numbers into different intervals. For example: p = [2 4 1 5 3] and k = 2, then we can put sticks at 2 positions  [2 4 | 1 | 5 3] between 4 and 5 to separate them into different intervals. For p = [1 4 5 7 3 2 6], and k = 3, then to separate 7 and 6 we can put it at 3 places [1 4 5 7 | 3 | 2 | 6], and to separate 5 and 7 we can similarly put it at one place.
```cpp
int n, k; cin >> n >> k;
vi a(n);
for(int i = 0; i < n; ++i) {
    cin >> a[i];
}
ll sum = 0;
for(int i = 0; i < k; ++i) {
    sum += n - i;
}
for(int i = 0; i < n; ++i) {
    if(a[i] > n - k) {
        a[i] = 0;
    }
}
for(int i = 0; i < n; ++i) {
    if(a[i] != 0) {
        a[i] = -1;
    } else {
        break;
    }
}
for(int i = n - 1; i >= 0; --i) {
    if(a[i] != 0) {
        a[i] = -1;
    } else {
        break;
    }
}
MI ans(1), positive_streak(0);
for(int i = 0; i < n; ++i) {
    if(a[i] == 0) {
        if(positive_streak == 0) {
            // first zero case
            continue;
        }
        ans *= positive_streak + 1;
        positive_streak = 0;
    } else if(a[i] > 0) {
        ++positive_streak;
    }
}
cout << sum << " " << ans << endl;
```

## [D1. Prefix-Suffix Palindrome (Easy version)](https://codeforces.com/contest/1326/problem/D1)
The idea was to match characters from both ends till they match and then find the longest palindromic prefix and suffix and see which forms the longest palindrome when attached to the macthed characters.
```cpp
bool is_palindrome(string& s, int start, int end) {
    wi(s.substr(start, end - start + 1))
    for(int i = start; i <= (start + end) / 2; ++i) {
        int partner = end - (i - start);
        if(s[i] != s[partner])
            return false;
    }
    return true;
}

int longest_palindromic_prefix(string& s, int start, int end) {
    int max_length = 0;
    for(int i = start; i <= end; ++i) {
        if(is_palindrome(s, start, i)) {
            max_length = max(i - start + 1, max_length);
        }
    }
    return max_length;
}

int longest_palindromic_suffix(string& s, int start, int end) {
    int max_length = 0;
    for(int i = end; i >= start; --i) {
        if(is_palindrome(s, i, end)) {
            max_length = max(end - i + 1, max_length);
        }
    }
    return max_length;
}

int main() {
    int t; cin >> t;
    while(t--) {
        string s; cin >> s; wi(s)
        int n = s.length();
        int match_cnt = 0;
        while(match_cnt < (n / 2) and  s[match_cnt] == s[n - match_cnt - 1]) {
            ++match_cnt;
        }
        int prefix = longest_palindromic_prefix(s, match_cnt, n - match_cnt - 1);
        int suffix = longest_palindromic_suffix(s, match_cnt, n - match_cnt - 1);
        if(prefix >= suffix) {
            cout << s.substr(0, match_cnt + prefix) << s.substr(n - match_cnt, match_cnt) << endl;
        } else {
            cout << s.substr(0, match_cnt) << s.substr(n - match_cnt - suffix, match_cnt + suffix) << endl;
        }
    }
}
```

## [D2. Prefix-Suffix Palindrome (Hard version)](https://codeforces.com/contest/1326/problem/D2)
The idea was to match characters from both ends till they match and then find the longest palindromic prefix and suffix and see which forms the longest palindrome when attached to the macthed characters.
In this hard version prefix function is used to obtain the longest plaindromic prefix and suffix. 

The prefix function for a string of length n is defined as an array π of length n, where π[i] is the length of the longest proper prefix of the substring s[0…i] which is also a suffix of this substring. 

So, if reverse(s) = s' and length of s#s' is n1. Then π[n1 - 1] would give the largest prefix which is also a suffix. Since, an # is included, this prefix can't have length longer than n. And since the prefix belongs to s and suffix belongs to s', and prefix = suffix, it is a palindrome.
```cpp
vi prefix_function(string s) {
    int n = s.length();
    vi pi(n);
    for (int i = 1; i < n; ++i) {
        int j = pi[i - 1];
        // invariant: j is the length of the prefix which matches the suffix at i - 1
        while (j > 0 && s[i] != s[j])
            j = pi[j - 1];
        if (s[i] == s[j])
            pi[i] = j + 1; // note that j is 0-indexed, so length is 1 more
        else
            pi[i] = j;
    }
    return pi;
}

int t; cin >> t;
while(t--) {
    string s; cin >> s; wi(s)
    int n = s.length();
    int match_cnt = 0;
    while(match_cnt < (n / 2) and  s[match_cnt] == s[n - match_cnt - 1]) {
        ++match_cnt;
    }
    string unmatched = s.substr(match_cnt, n - 2 * match_cnt);
    string unmatched_reverse = string(unmatched.rbegin(), unmatched.rend());
    string fwd = unmatched + '#' + unmatched_reverse;
    string bwd = unmatched_reverse + '#' + unmatched;
    int lpp = prefix_function(fwd)[fwd.length() - 1]; // longest palindromic prefix
    int lps = prefix_function(bwd)[bwd.length() - 1]; // longest palindromic suffix
    if(lpp >= lps)
        cout << s.substr(0, match_cnt + lpp) << s.substr(n - match_cnt, match_cnt) << endl;
    else
        cout << s.substr(0, match_cnt) << s.substr(n - match_cnt - lps, match_cnt + lps) << endl;
```
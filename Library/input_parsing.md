## Reading till EOF
```cpp
string line;
while (getline(cin, line)) {
    istringstream line_stream(line);
    int Ar, Ac, Tr, Tc; line_stream >> Ar >> Ac >> Tr >> Tc;
}
```

## Get lines ignoring first new line
Get lines ignoring everything till the first new line (space also have to be captured). Useful in the following scenario.
```
// input
*. *. **
.. *. ..
.. .. ..
```
```cpp
// This wouldn't work.
char c; cin >> c;
vector<string> vs;
for(int i = 0; i < 3; ++i)
    getline(cin, vs[i]);
```
```cpp
// This works.
vs get_lines(int rows) {
    cin.ignore(numeric_limits<streamsize>::max(), '\n');
    vs input(rows);
    for(int i = 0; i < rows; ++i) {
        getline(cin, input[i]);
    }
    return input;
}
```

## Input separated by empty lines, number of test cases specified
```
2

10111000
00010100
00111000
00111010
00111111
01011110
01011110
00011110

10111000
00010100
00111000
00111010
00111111
01011110
01011110
00011110
```
```cpp
int t; cin >> t;
string line; getline(cin, line); getline(cin, line);
while(t--) {
    vector<string> input;
    while(true) {
        string line; getline(cin, line);
        if(line.empty()) break;
        input.pb(line);
    }
    print(input);
}
```

## Input separated by empty lines, number of test cases not specified
N M<br>
N x N matrix, with each cell in one line<br>
Two input separated by new line<br>
```
4 3
1
2
2
1
1
0
2
2
0
2
3
3
1
2
1
3

5 2
1
0
1
0
0
0
0
1
0
0
1
1
1
1
1
0
0
1
0
0
0
0
1
0
0
```
```cpp
while (true) {
    string line;
    while(line.empty()) { // gobble up new lines till a non-empty input
        if(!getline(cin, line))
            exit(0); // EOF
    }
    istringstream line_stream(line);
    int n, m; line_stream >> n >> m; wi(n, m)
    vvi a(n + 2, vi(n + 2));
    for(int i = n; i >= 1; --i) {
        for(int j = 1; j <= n; ++j) {
            cin >> a[i][j];
        }
    }
}
```
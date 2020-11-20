```cpp
int t;
scanf("%d\n", &t);
while (t--) {
    char piece;
    int m, n;
    scanf("%c %d %d\n", &piece, &m, &n);
    switch (piece) {
    case 'r':
    case 'Q':
        printf("%d\n", min(m, n));
        break;
    case 'K':
        printf("%d\n", (int) (ceil(m / 2.0) * ceil(n / 2.0)));
        break;
    case 'k':
        printf("%d\n",
                (int) (ceil(m / 2.0) * ceil(n / 2.0)
                        + floor(m / 2.0) * floor(n / 2.0)));
    }
}
```
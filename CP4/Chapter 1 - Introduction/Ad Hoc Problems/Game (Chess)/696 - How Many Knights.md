```cpp
int num_knights(int m, int n) {
    int x = ((int) (ceil(m / 2.0) * ceil(n / 2.0)
            + floor(m / 2.0) * floor(n / 2.0)));
    if (m >= 3 && n >= 3)
        return x;
    else if (m == 1 || n == 1)
        return m * n;
    else if (m <= 0 || n <= 0)
        return 0;
    else {
        int y;
        int k = (m != 2 ? m : n);
        if (k % 4 == 0) {
            y = k;
        } else if (k % 4 == 1) {
            y = 2 + (k - 1);
        } else if (k % 4 == 2) {
            y = k + 2;
        } else if (k % 4 == 3) {
            y = k + 1;
        }
        return max(y, x);
    }
}
int main() {
    //freopen("input.txt", "r", stdin);
    //freopen("output.txt", "w", stdout);
    while (1) {
        int m, n;
        scanf("%d %d\n", &m, &n);
        if (m == 0 && n == 0)
            break;
        printf("%d knights may be placed on a %d row %d column board.\n",
                num_knights(m, n), m, n);

    }
    return 0;
}
```
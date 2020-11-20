```cpp
int t;
scanf("%d\n\n", &t);
while (t--) {
    int m, n;
    scanf("%d\n%d\n", &m, &n);
    while(m--) {
        int r1, c1, r2, c2;
        scanf("%d %d %d %d\n", &r1, &c1, &r2, &c2);
        if(r1 == r2 && c1 == c2)
            printf("0\n");
        else if((r1 + c1) % 2 != (r2+c2)%2) {
            printf("no move\n");
        } else if(fabs(c2 - c1) == fabs(r2 -r1)) {
            printf("1\n");
        } else {
            printf("2\n");
        }
    }
}
```
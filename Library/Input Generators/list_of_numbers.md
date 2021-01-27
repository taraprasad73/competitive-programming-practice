# Using effolkronium/random library
https://github.com/effolkronium/random

# numbers between [-INF32, RAND_MAX]
```cpp
std::srand(std::time(nullptr)); // use current time as seed for random generator
int random_variable = std::rand();
for(int i = 0; i < 10000; ++i) {
    cout << (-INF32 + std::rand()) << endl;
}
```
```cpp
struct cell_t {
    bool occupied = false;
    bool reachableByQueen = false;
    bool reachableByKing = false;
    friend ostream& operator<<(ostream& os, const cell_t& c);
};
ostream& operator<<(ostream& os, const cell_t& c) {
    os << "{" << c.occupied << "," << c.reachableByKing << "," << c.reachableByQueen << "}";
    return os;
}
```
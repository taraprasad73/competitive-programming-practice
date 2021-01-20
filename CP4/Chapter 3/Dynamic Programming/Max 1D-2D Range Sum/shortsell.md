Official Solution: https://github.com/Kodsport/liu-coding-challenge-2018/blob/master/shortsell/submissions/accepted/js.cpp
```cpp
/**
 * After selling the stocks on some day (lets say x), we will get some amount (referred as amountInHand).
 * This amount will decrease every day due to interest deduction.
 * Every day, we have to make a choice whether to continue with the current decision, i.e,
 * selling the stocks on day x, or discard that choice and sell in current day instead. 
 * We do the latter if on selling, the amount received is greater than the amountInHand.
 * The logic is greedy, similar to Kadane, i.e., it is better to continue with a high amount in hand.  
 */ 
void solve() {
    ll quantity = 100;
    int n, k; cin >> n >> k;
    ll maxProfit = 0, amountInHand = 0;
    for(int i = 0; i < n; ++i) {
        ll price; cin >> price;
        if((price * quantity) > amountInHand) {
            // sell the stocks on this day, discard previous choices
            amountInHand = price * quantity;
        }
        amountInHand -= k; // deduct the interest
        // take a decision whether to buy back the stocks or not
        ll profit = amountInHand - (price * quantity);
        maxProfit = max(profit, maxProfit);
    }
    cout << maxProfit << endl;
}
```
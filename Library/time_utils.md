# Log execution time
```cpp
clock_t start_time = clock();
void logTime() {
    float time_taken = (float)(clock() - start_time) / CLOCKS_PER_SEC;
    LOG("Time Taken: " + to_string(time_taken) + "s")
}
```

# Timer to run periodically
Helpful when running a long running precomputation task as in kattis foolingaround. Need -lpthread in gcc args, so don't send it to online judge. 
https://stackoverflow.com/questions/30425772/c-11-calling-a-c-function-periodically
https://stackoverflow.com/questions/1662909/undefined-reference-to-pthread-create-in-linux
```cpp
class CallBackTimer {
public:
    CallBackTimer() :_execute(false) {}

    ~CallBackTimer() {
        if( _execute.load(std::memory_order_acquire) ) {
            stop();
        };
    }

    void stop() {
        _execute.store(false, std::memory_order_release);
        if( _thd.joinable() )
            _thd.join();
    }

    void start(int interval, std::function<void(void)> func) {
        if( _execute.load(std::memory_order_acquire) ) {
            stop();
        };
        _execute.store(true, std::memory_order_release);
        _thd = std::thread([this, interval, func]()
        {
            while (_execute.load(std::memory_order_acquire)) {
                func();                   
                std::this_thread::sleep_for(
                std::chrono::milliseconds(interval));
            }
        });
    }

    bool is_running() const noexcept {
        return ( _execute.load(std::memory_order_acquire) && 
                 _thd.joinable() );
    }

private:
    std::atomic<bool> _execute;
    std::thread _thd;
};
```
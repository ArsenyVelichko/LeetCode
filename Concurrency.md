# Concurrency

+ [Print in Order](#print-in-order)

## Print in Order

```C++
class Foo {
public:
    Foo() {
        
    }

    void first(function<void()> printFirst) {
        unique_lock<mutex> locker(m_lock);
        
        m_condVar.wait(locker, [this] { return m_currIdx == 1; });
        
        // printFirst() outputs "first". Do not change or remove this line.
        printFirst();
        m_currIdx = 2;
        m_condVar.notify_all();
    }

    void second(function<void()> printSecond) {
        unique_lock<mutex> locker(m_lock);
        
        m_condVar.wait(locker, [this] { return m_currIdx == 2; });
        
        // printSecond() outputs "second". Do not change or remove this line.
        printSecond();
        m_currIdx = 3;
        m_condVar.notify_all();
    }

    void third(function<void()> printThird) {
        unique_lock<mutex> locker(m_lock);
        
        m_condVar.wait(locker, [this] { return m_currIdx == 3; });
        
        // printThird() outputs "third". Do not change or remove this line.
        printThird();
        m_currIdx = 1;
        m_condVar.notify_all();
    }
    
private:
    size_t m_currIdx = 1;
    condition_variable m_condVar;
    mutex m_lock;
};
```

# মিউটেক্স ও সিঙ্ক (Mutex & Sync)

যখন একাধিক গোরুটিন একই সাথে কোনো শেয়ার্ড ভ্যারিয়েবল বা মেমোরিতে লিখতে বা পড়তে চায়, তখন রেস কন্ডিশন (Race Condition) তৈরি হতে পারে। এর ফলে ডেটা নষ্ট বা ভুল ফলাফল দেখা দেয়। এই ধরনের সমস্যা সমাধানের জন্য গো-এর `sync` প্যাকেজের **Mutex (Mutual Exclusion)** ব্যবহার করা হয়।

---

## ১. sync.Mutex দিয়ে রেস কন্ডিশন প্রতিরোধ

`sync.Mutex` ব্যবহার করে নিশ্চিত করা যায় যে যেকোনো মুহূর্তে শুধুমাত্র একটি গোরুটিনই ক্রিটিকাল কোড অংশটি এক্সিকিউট করতে পারবে:

```go
package main

import (
    "fmt"
    "sync"
)

type SafeCounter struct {
    mu    sync.Mutex
    count int
}

func (c *SafeCounter) Inc() {
    c.mu.Lock()
    defer c.mu.Unlock()
    c.count++
}

func (c *SafeCounter) Value() int {
    c.mu.Lock()
    defer c.mu.Unlock()
    return c.count
}

func main() {
    counter := SafeCounter{}
    var wg sync.WaitGroup

    // ১০০০টি গোরুটিন সমান্তরালভাবে কাউন্টার বাড়াবে
    for range 1000 {
        wg.Add(1)
        go func() {
            defer wg.Done()
            counter.Inc()
        }()
    }

    wg.Wait()
    fmt.Println("চূড়ান্ত কাউন্টার মান:", counter.Value())
}
```

আউটপুট:

```text
চূড়ান্ত কাউন্টার মান: 1000
```

> **টিপ:** রেস কন্ডিশন আছে কিনা পরীক্ষা করতে গো-তে রেস ডিটেক্টর রয়েছে। টার্মিনালে `go run -race .` বা `go test -race` কমান্ড দিয়ে স্বয়ংক্রিয়ভাবে রেস কন্ডিশন ধরা যায়।

---

## ২. sync.RWMutex (রিড/রাইট মিউটেক্স)

যদি সিস্টেমে লেখার চেয়ে পড়ার সংখ্যা অনেক বেশি হয় (Read-heavy operations), তখন `sync.RWMutex` ব্যবহার করা হয়। এতে একাধিক গোরুটিন একসাথে পড়তে পারে (`RLock`), কিন্তু লেখার সময় শুধুমাত্র একজনই এক্সক্লুসিভ লক (`Lock`) পায়:

```go
package main

import (
    "fmt"
    "sync"
)

type ConfigStore struct {
    rw    sync.RWMutex
    value string
}

func (c *ConfigStore) Get() string {
    c.rw.RLock()
    defer c.rw.RUnlock()
    return c.value
}

func (c *ConfigStore) Set(val string) {
    c.rw.Lock()
    defer c.rw.Unlock()
    c.value = val
}

func main() {
    store := ConfigStore{}
    store.Set("v1.0.0")
    fmt.Println("কনফিগ ভ্যালু:", store.Get())
}
```

আউটপুট:

```text
কনফিগ ভ্যালু: v1.0.0
```

---

## ৩. sync.Once

কোনো ইনিশিয়ালাইজেশন কোড (যেমন ডাটাবেস কানেকশন সেটআপ বা কনফিগ লোড) শত শত গোরুটিন কল করলেও যেন ঠিক একবারই রান হয়, তা নিশ্চিত করতে `sync.Once` ব্যবহৃত হয়:

```go
package main

import (
    "fmt"
    "sync"
)

func main() {
    var once sync.Once
    var wg sync.WaitGroup

    initFunc := func() {
        fmt.Println("ডাটাবেস কানেকশন ইনিশিয়ালাইজ করা হলো (একবারই রান হবে)")
    }

    // ৩টি গোরুটিন কল করলেও initFunc কেবল একবারই রান হবে
    for range 3 {
        wg.Add(1)
        go func() {
            defer wg.Done()
            once.Do(initFunc)
        }()
    }

    wg.Wait()
}
```

আউটপুট:

```text
ডাটাবেস কানেকশন ইনিশিয়ালাইজ করা হলো (একবারই রান হবে)
```

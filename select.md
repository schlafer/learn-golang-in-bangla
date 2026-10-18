# সিলেক্ট (Select)

গো-তে একাধিক চ্যানেলের জন্য একসাথে অপেক্ষা করতে এবং যে চ্যানেলটি প্রথমে ডেটা প্রদান করবে সেটির কাজ সম্পন্ন করতে `select` স্টেটমেন্ট ব্যবহার করা হয়। এটি দেখতে সাধারণ `switch` এর মতো হলেও এটি কাজ করে চ্যানেলের কমিউনিকেশনের উপর ভিত্তি করে।

---

## ১. একাধিক চ্যানেল হ্যান্ডেল করা

নিচের উদাহরণে দুটি ভিন্ন চ্যানেল থেকে মেসেজ আসার জন্য `select` অপেক্ষা করে:

```go
package main

import (
    "fmt"
    "time"
)

func main() {
    c1 := make(chan string)
    c2 := make(chan string)

    go func() {
        time.Sleep(10 * time.Millisecond)
        c1 <- "সার্ভিস ১ থেকে মেসেজ"
    }()

    go func() {
        time.Sleep(20 * time.Millisecond)
        c2 <- "সার্ভিস ২ থেকে মেসেজ"
    }()

    // ২টি মেসেজের জন্য select
    for i := 0; i < 2; i++ {
        select {
        case msg1 := <-c1:
            fmt.Println("প্রাপ্ত:", msg1)
        case msg2 := <-c2:
            fmt.Println("প্রাপ্ত:", msg2)
        }
    }
}
```

আউটপুট:

```text
প্রাপ্ত: সার্ভিস ১ থেকে মেসেজ
প্রাপ্ত: সার্ভিস ২ থেকে মেসেজ
```

---

## ২. টাইমআউট হ্যান্ডেল করা (`time.After`)

নেটওয়ার্ক বা বাহ্যিক সার্ভার থেকে সময়মতো ডেটা না আসলে প্রোগ্রাম যাতে অনির্দিষ্টকাল আটকে (hang) না থাকে, সেজন্য `time.After` দিয়ে টাইমআউট সেট করা হয়:

```go
package main

import (
    "fmt"
    "time"
)

func main() {
    c := make(chan string)

    go func() {
        time.Sleep(50 * time.Millisecond) // ডেটা আসতে ৫০ms লাগবে
        c <- "বিলম্বিত ফলাফল"
    }()

    select {
    case res := <-c:
        fmt.Println("ফলাফল:", res)
    case <-time.After(20 * time.Millisecond): // কিন্তু আমরা সর্বোচ্চ ২০ms অপেক্ষা করব
        fmt.Println("টাইমআউট! নির্দিষ্ট সময়ে রেসপন্স আসেনি।")
    }
}
```

আউটপুট:

```text
টাইমআউট! নির্দিষ্ট সময়ে রেসপন্স আসেনি।
```

---

## ৩. নন-ব্লকিং চ্যানেল অপারেশন (`default`)

যদি চ্যানেলে ডেটা অবিলম্বে প্রস্তুত না থাকে তবে অপেক্ষা না করে সরাসরি অন্য কাজে চলে যেতে `default` কেস ব্যবহার করা হয়:

```go
package main

import "fmt"

func main() {
    messages := make(chan string)

    select {
    case msg := <-messages:
        fmt.Println("মেসেজ পাওয়া গেছে:", msg)
    default:
        fmt.Println("চ্যানেলে কোনো মেসেজ নেই, অপেক্ষা না করে এগিয়ে যাচ্ছি।")
    }
}
```

আউটপুট:

```text
চ্যানেলে কোনো মেসেজ নেই, অপেক্ষা না করে এগিয়ে যাচ্ছি।
```

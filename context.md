# কনটেক্সট (Context)

গো প্রোগ্রামিংয়ে কনটেক্সট (`context.Context`) হলো যেকোনো নেটওয়ার্ক রিকোয়েস্ট, ডেটাবেস কুয়েরি এবং কনকারেন্ট পাইপলাইনের লাইফসাইকেল পরিচালনার অন্যতম প্রধান ভিত্তি।

যখন কোনো ক্লায়েন্ট ব্রাউজার বন্ধ করে দেয় বা কোনো রিকোয়েস্ট নির্দিষ্ট সময়ের চেয়ে বেশি দেরি করে, তখন যাতে ব্যাকগ্রাউন্ডের অপ্রয়োজনীয় গোরুটিনগুলো থেমে যায় এবং রিসোর্স অপচয় বন্ধ হয়, সেটার জন্যই `context` ব্যবহৃত হয়।

---

## ১. ক্যান্সেলেশন (context.WithCancel)

একটি অভিভাবক (Parent) কনটেক্সট থেকে চাইল্ড কনটেক্সট তৈরি করে যখনই `cancel()` কল করা হয়, সংশ্লিষ্ট সব গোরুটিনের `<-ctx.Done()` চ্যানেল সক্রিয় হয়ে ওঠে:

```go
package main

import (
    "context"
    "fmt"
    "sync"
    "time"
)

func worker(ctx context.Context, wg *sync.WaitGroup) {
    defer wg.Done()
    for {
        select {
        case <-ctx.Done():
            fmt.Println("ক্যান্সেলেশন সিগন্যাল পাওয়া গেছে! গোরুটিন থামানো হচ্ছে।")
            return
        default:
            fmt.Println("কাজ চলছে...")
            time.Sleep(20 * time.Millisecond)
        }
    }
}

func main() {
    // ক্যান্সেলেশন কনটেক্সট তৈরি
    ctx, cancel := context.WithCancel(context.Background())
    var wg sync.WaitGroup

    wg.Add(1)
    go worker(ctx, &wg)

    time.Sleep(50 * time.Millisecond)
    cancel() // গোরুটিন বন্ধের নির্দেশ পাঠানো হলো
    wg.Wait() // গোরুটিনের কাজ সুষ্ঠুভাবে শেষ হওয়া পর্যন্ত অপেক্ষা
}
```

আউটপুট:

```text
কাজ চলছে...
কাজ চলছে...
কাজ চলছে...
ক্যান্সেলেশন সিগন্যাল পাওয়া গেছে! গোরুটিন থামানো হচ্ছে।
```

---

## ২. টাইমআউট এবং ডেডলাইন (context.WithTimeout)

নির্দিষ্ট সময় পর স্বয়ংক্রিয়ভাবে অপারেশন বাতিল করতে `context.WithTimeout` ব্যবহার করা হয়:

```go
package main

import (
    "context"
    "fmt"
    "time"
)

func queryDatabase(ctx context.Context) {
    select {
    case <-time.After(100 * time.Millisecond): // কুয়েরি শেষ হতে ১০০ms লাগে
        fmt.Println("কুয়েরি সফল হয়েছে")
    case <-ctx.Done(): // কিন্তু আমরা সর্বোচ্চ ৩০ms সময় দিয়েছি
        fmt.Println("কুয়েরি বাতিল হয়েছে:", ctx.Err())
    }
}

func main() {
    // ৩০ মিলিসেকেন্ডের টাইমআউট কনটেক্সট
    ctx, cancel := context.WithTimeout(context.Background(), 30*time.Millisecond)
    defer cancel()

    queryDatabase(ctx)
}
```

আউটপুট:

```text
কুয়েরি বাতিল হয়েছে: context deadline exceeded
```

---

## ৩. রিকোয়েস্ট স্কোপড ভ্যালু পাস করা (context.WithValue)

এইচটিটিপি রিকোয়েস্টের মধ্য দিয়ে ট্রেস আইডি (Trace ID) বা ইউজার ক্রেডেনশিয়ালের মতো রিকোয়েস্ট-স্কোপড মেটাডেটা পাস করতে `context.WithValue` ব্যবহৃত হয়:

```go
package main

import (
    "context"
    "fmt"
)

type contextKey string

func processRequest(ctx context.Context) {
    // কনটেক্সট থেকে ভ্যালু পড়া
    if traceID, ok := ctx.Value(contextKey("trace_id")).(string); ok {
        fmt.Println("প্রসেসিং রিকোয়েস্ট Trace ID:", traceID)
    }
}

func main() {
    ctx := context.WithValue(context.Background(), contextKey("trace_id"), "req-abc-12345")
    processRequest(ctx)
}
```

আউটপুট:

```text
প্রসেসিং রিকোয়েস্ট Trace ID: req-abc-12345
```

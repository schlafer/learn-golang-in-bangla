# কন্সট্যান্টস

যেসব ভ্যারিয়েবলের মান প্রোগ্রাম চলার সময় কখনো পরিবর্তন হয় না বা অপরিবর্তনীয় রাখতে চাই, সেগুলোর জন্য গো-তে `const` কি-ওয়ার্ড ব্যবহার করা হয়।

কনস্ট্যান্ট সাধারণত ক্যারেক্টার, স্ট্রিং, বুলিয়ান অথবা নিউমেরিক ভ্যালু হয়ে থাকে। 

```go
package main

import "fmt"

// প্যাকেজ লেভেলে কনস্ট্যান্ট ডিক্লেয়ারেশন
const appName string = "GoLearningApp"

func main() {
    fmt.Println("App:", appName)

    const maxUsers = 500000
    fmt.Println("Max Users:", maxUsers)
}
```

আউটপুট:

```text
App: GoLearningApp
Max Users: 500000
```

---

## কনস্ট্যান্ট গ্রুপিং এবং iota

একাধিক কনস্ট্যান্ট একসাথে গ্রুপ আকারে ডিক্লেয়ার করা যায়। এছাড়া ক্রমান্বয়ে বৃদ্ধি পাওয়া সংখ্যার তালিকা (যেমন সপ্তাহের দিন বা স্ট্যাটাস কোড) তৈরি করতে গো-তে রয়েছে অত্যন্ত চমৎকার বিল্টইন টুল `iota`:

```go
package main

import "fmt"

const (
    StatusPending = iota // ০
    StatusActive         // ১
    StatusBlocked        // ২
)

func main() {
    fmt.Println("Pending:", StatusPending)
    fmt.Println("Active:", StatusActive)
    fmt.Println("Blocked:", StatusBlocked)
}
```

আউটপুট:

```text
Pending: 0
Active: 1
Blocked: 2
```

`iota` ব্যবহারের ফলে প্রতি লাইনে স্বয়ংক্রিয়ভাবে মান এক করে বেড়ে যায়, যা গো কোডকে অত্যন্ত পরিচ্ছন্ন রাখে।

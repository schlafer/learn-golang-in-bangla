# সুইচ (Switch)

গো-তে একাধিক শর্ত সহজে প্রকাশ করতে `switch` স্টেটমেন্ট ব্যবহার করা হয়। অন্যান্য অনেক ভাষার মতো গো-তে প্রতিটি কেইসের শেষে স্বয়ংক্রিয়ভাবে `break` কার্যকর হয়—অর্থাৎ কোনো কেইস মিললে স্বয়ংক্রিয়ভাবে নিচে গড়িয়ে পড়ে না (fallthrough হয় না)।

---

## ১. সাধারণ মানভিত্তিক সুইচ

```go
package main

import "fmt"

func main() {
    i := 2
    fmt.Print("সংখ্যা ", i, " বাংলায়: ")

    switch i {
    case 1:
        fmt.Println("এক")
    case 2:
        fmt.Println("দুই")
    case 3:
        fmt.Println("তিন")
    default:
        fmt.Println("অন্য সংখ্যা")
    }
}
```

আউটপুট:

```text
সংখ্যা 2 বাংলায়: দুই
```

---

## ২. কমা দিয়ে একাধিক শর্ত এবং শর্তহীন সুইচ

একটি কেইসেই কমা দিয়ে একাধিক মান পরীক্ষা করা যায়। আবার `switch` এর পর কোনো এক্সপ্রেশন না দিলে এটি পরিচ্ছন্ন `if-else` এর মতো কাজ করে:

```go
package main

import (
    "fmt"
    "time"
)

func main() {
    // কমা দিয়ে একাধিক শর্ত
    switch time.Now().Weekday() {
    case time.Friday, time.Saturday:
        fmt.Println("আজ ছুটির দিন!")
    default:
        fmt.Println("আজ কর্মদিবস")
    }

    // শর্তহীন সুইচ (if-else এর পরিচ্ছন্ন রূপ)
    hour := 14
    switch {
    case hour < 12:
        fmt.Println("শুভ সকাল")
    case hour < 18:
        fmt.Println("শুভ অপরাহ্ন")
    default:
        fmt.Println("শুভ সন্ধ্যা")
    }
}
```

আউটপুট:

```text
আজ কর্মদিবস
শুভ অপরাহ্ন
```

---

## ৩. টাইপ সুইচ (Type Switch)

কোনো ইন্টারফেস বা `any` টাইপের ভ্যারিয়েবলের ভেতরের আসল টাইপ পরীক্ষা করতে গো-তে রয়েছে শক্তিশালী টাইপ সুইচ:

```go
package main

import "fmt"

func checkType(i any) {
    switch v := i.(type) {
    case bool:
        fmt.Println("এটি একটি বুলিয়ান:", v)
    case int:
        fmt.Println("এটি একটি পূর্ণসংখ্যা:", v*2)
    case string:
        fmt.Println("এটি একটি স্ট্রিং:", v)
    default:
        fmt.Printf("অজানা টাইপ %T\n", v)
    }
}

func main() {
    checkType(true)
    checkType(42)
    checkType("গো ল্যাঙ্গুয়েজ")
}
```

আউটপুট:

```text
এটি একটি বুলিয়ান: true
এটি একটি পূর্ণসংখ্যা: 84
এটি একটি স্ট্রিং: গো ল্যাঙ্গুয়েজ
```

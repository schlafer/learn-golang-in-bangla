# এরর হ্যান্ডলিং

গো-তে এরর হ্যান্ডলিংয়ের দর্শন অন্যান্য ল্যাঙ্গুয়েজের (যেমন জাভা বা পাইথনের `try/catch/throw`) চেয়ে সম্পূর্ণ আলাদা। গো-তে কোনো এক্সেপশন মেকানিজম নেই—বরং **"Errors are values"** (এরর হলো সাধারণ ভ্যালুর মতো)। কোনো অপারেশন ব্যর্থ হতে পারলে ফাংশনটি তার শেষ রিটার্ন ভ্যালু হিসেবে একটি `error` রিটার্ন করে।

---

## ১. এরর তৈরি এবং বেসিক চেকিং

গো-তে বিল্টইন `error` একটি সাধারণ ইন্টারফেস:
```go
type error interface {
    Error() string
}
```

সাধারণ এরর তৈরি করতে `errors.New` অথবা ফরম্যাটিং সহ তৈরি করতে `fmt.Errorf` ব্যবহার করা হয়:

```go
package main

import (
    "errors"
    "fmt"
)

// ভাগ করার ফাংশন যা শূন্য দিয়ে ভাগ করার ক্ষেত্রে এরর রিটার্ন করে
func divide(a, b float64) (float64, error) {
    if b == 0 {
        return 0, errors.New("শূন্য দিয়ে ভাগ করা সম্ভব নয়")
    }
    return a / b, nil
}

func main() {
    result, err := divide(10, 2)
    if err != nil {
        fmt.Println("এরর:", err)
    } else {
        fmt.Println("ভাগফল:", result)
    }

    _, err = divide(10, 0)
    if err != nil {
        fmt.Println("এরর:", err)
    }
}
```

আউটপুট:

```text
ভাগফল: 5
এরর: শূন্য দিয়ে ভাগ করা সম্ভব নয়
```

---

## ২. আধুনিক এরর র‍্যাপিং (`%w`) এবং errors.Is / errors.As

আধুনিক গো-তে একটি এররকে অন্য কনটেক্সটের ভেতরে মুড়িয়ে (wrapping) পাঠাতে `fmt.Errorf` এর `%w` ভার্ব ব্যবহার করা হয়। আর এররটি আসল সেন্টিনেল এরর কিনা তা চেক করতে `errors.Is` এবং নির্দিষ্ট কাস্টম এরর টাইপ কিনা তা জানতে `errors.As` ব্যবহৃত হয়:

```go
package main

import (
    "errors"
    "fmt"
)

// সেন্টিনেল এরর
var ErrNotFound = errors.New("ডাটা পাওয়া যায়নি")

func findUser(id int) error {
    if id != 1 {
        // %w দিয়ে ErrNotFound র‍্যাপ করা হলো
        return fmt.Errorf("user query failed for id %d: %w", id, ErrNotFound)
    }
    return nil
}

func main() {
    err := findUser(99)
    if err != nil {
        fmt.Println("সম্পূর্ণ এরর মেসেজ:", err)

        // errors.Is দিয়ে মূল কারণ যাচাই
        if errors.Is(err, ErrNotFound) {
            fmt.Println("-> এটি একটি ErrNotFound এরর!")
        }
    }
}
```

আউটপুট:

```text
সম্পূর্ণ এরর মেসেজ: user query failed for id 99: ডাটা পাওয়া যায়নি
-> এটি একটি ErrNotFound এরর!
```

---

## ৩. একাধিক এরর একত্রিত করা: errors.Join

একাধিক ফিল্ড ভ্যালিডেশনের ক্ষেত্রে সব এররকে একটি একক এররে একত্রিত করতে আধুনিক গো-তে `errors.Join` ব্যবহার করা হয়:

```go
package main

import (
    "errors"
    "fmt"
)

func validateForm(name string, age int) error {
    var errs []error
    if name == "" {
        errs = append(errs, errors.New("নাম ফাঁকা হতে পারবে না"))
    }
    if age < 18 {
        errs = append(errs, errors.New("বয়স অন্তত ১৮ হতে হবে"))
    }
    return errors.Join(errs...)
}

func main() {
    err := validateForm("", 16)
    if err != nil {
        fmt.Println("ভ্যালিডেশন এররসমূহ:\n", err)
    }
}
```

আউটপুট:

```text
ভ্যালিডেশন এররসমূহ:
 নাম ফাঁকা হতে পারবে না
বয়স অন্তত ১৮ হতে হবে
```

# ইন্টারফেস

গো-তে ইন্টারফেস (Interface) হলো মেথড সিগনেচারের একটি সেট বা চুক্তি (Contract)। গো-তে ইন্টারফেস বাস্তবায়ন করার জন্য কোনো বিশেষ কি-ওয়ার্ড (যেমন জাভার `implements`) লিখতে হয় না—কোনো স্ট্রাক্ট যদি ইন্টারফেসের নির্ধারিত মেথডগুলো বাস্তবায়ন করে, তবে গো স্বয়ংক্রিয়ভাবে ধরে নেয় যে টাইপটি ওই ইন্টারফেস বাস্তবায়ন করেছে (Duck typing: "If it walks like a duck and quacks like a duck, it's a duck")।

---

## ১. ইন্টারফেস এবং পলিমরফিজম (Polymorphism)

```go
package main

import (
    "fmt"
    "math"
)

// Shape ইন্টারফেস: যেকোনো টাইপ যাতে Area() মেথড আছে
type Shape interface {
    Area() float64
}

// Rectangle স্ট্রাক্ট
type Rectangle struct {
    Width, Height float64
}

func (r Rectangle) Area() float64 {
    return r.Width * r.Height
}

// Circle স্ট্রাক্ট
type Circle struct {
    Radius float64
}

func (c Circle) Area() float64 {
    return math.Pi * c.Radius * c.Radius
}

// জেনেরিক প্রিন্টার যা যেকোনো Shape গ্রহণ করে
func printArea(s Shape) {
    fmt.Printf("ক্ষেত্রফল: %.2f\n", s.Area())
}

func main() {
    rect := Rectangle{Width: 10, Height: 5}
    circ := Circle{Radius: 3}

    printArea(rect)
    printArea(circ)
}
```

আউটপুট:

```text
ক্ষেত্রফল: 50.00
ক্ষেত্রফল: 28.27
```

---

## ২. empty interface এবং any

গো-তে `any` (পূর্বে `interface{}`) হলো এমন একটি ইন্টারফেস যাতে কোনো মেথড নেই। ফলে যেকোনো টাইপের ডেটা (int, string, struct) `any` এর মধ্যে রাখা যায়:

```go
package main

import "fmt"

func printUnknown(val any) {
    fmt.Println("Value:", val)
}

func main() {
    printUnknown(42)
    printUnknown("গো ল্যাঙ্গুয়েজ")
    printUnknown(true)
}
```

আউটপুট:

```text
Value: 42
Value: গো ল্যাঙ্গুয়েজ
Value: true
```

---

## ৩. টাইপ অ্যাসারশন ও টাইপ সুইচ

`any` বা ইন্টারফেসের ভেতরের আসল কংক্রিট টাইপ জানতে টাইপ অ্যাসারশন `val, ok := i.(Type)` অথবা `switch i.(type)` ব্যবহার করা হয়:

```go
package main

import "fmt"

func describeType(i any) {
    switch v := i.(type) {
    case int:
        fmt.Println("এটি একটি পূর্ণসংখ্যা:", v*2)
    case string:
        fmt.Println("এটি একটি স্ট্রিং:", v)
    default:
        fmt.Println("অন্যান্য অজানা টাইপ")
    }
}

func main() {
    // সরাসরি টাইপ অ্যাসারশন (Comma-ok idiom)
    var val any = "হ্যালো গো"
    str, ok := val.(string)
    if ok {
        fmt.Println("সফলভাবে স্ট্রিং পাওয়া গেছে:", str)
    }

    // টাইপ সুইচ
    describeType(25)
    describeType("বাংলা টিউটোরিয়াল")
}
```

আউটপুট:

```text
সফলভাবে স্ট্রিং পাওয়া গেছে: হ্যালো গো
এটি একটি পূর্ণসংখ্যা: 50
এটি একটি স্ট্রিং: বাংলা টিউটোরিয়াল
```

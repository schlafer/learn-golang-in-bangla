# মেথড

গো-তে মেথড (Method) হলো এমন একটি বিশেষ ফাংশন, যা কোনো নির্দিষ্ট টাইপের (সাধারণত স্ট্রাক্ট) সাথে যুক্ত থাকে। মেথড ডিফাইন করার সময় ফাংশনের নামের আগে একটি "রিসিভার" (Receiver) প্যারামিটার যুক্ত করতে হয়।

---

## ১. মেথড ডিক্লেয়ার করা

সিনট্যাক্স: `func (r ReceiverType) MethodName(params) ReturnType`

```go
package main

import "fmt"

type Rectangle struct {
    Width, Height float64
}

// Rectangle টাইপের একটি মেথড Area
func (r Rectangle) Area() float64 {
    return r.Width * r.Height
}

// Rectangle টাইপের আরেকটি মেথড Perimeter
func (r Rectangle) Perimeter() float64 {
    return 2 * (r.Width + r.Height)
}

func main() {
    rect := Rectangle{Width: 10, Height: 5}

    // ডট দিয়ে অবজেক্টের মেথড কল করা
    fmt.Println("আয়তক্ষেত্রের ক্ষেত্রফল:", rect.Area())
    fmt.Println("আয়তক্ষেত্রের পরিসীমা:", rect.Perimeter())
}
```

আউটপুট:

```text
আয়তক্ষেত্রের ক্ষেত্রফল: 50
আয়তক্ষেত্রের পরিসীমা: 30
```

---

## ২. ভ্যালু রিসিভার বনাম পয়েন্টার রিসিভার

গো-তে মেথড দুই ধরনের রিসিভার গ্রহণ করতে পারে:

1. **ভ্যালু রিসিভার (Value Receiver):** মূল অবজেক্টের একটি কপি নিয়ে কাজ করে। এর ভেতরে কোনো পরিবর্তন মূল অবজেক্টে প্রভাব ফেলে না।
2. **পয়েন্টার রিসিভার (Pointer Receiver):** মূল অবজেক্টের মেমোরি পয়েন্টার গ্রহণ করে। এর ফলে মেথডের ভেতর থেকে ফিল্ডের মান পরিবর্তন করা যায় এবং বড় স্ট্রাক্ট বারবার কপি হওয়া রোধ হয়।

```go
package main

import "fmt"

type Counter struct {
    Count int
}

// ভ্যালু রিসিভার: মূল মান পরিবর্তন করতে পারে না
func (c Counter) IncrementVal() {
    c.Count++
}

// পয়েন্টার রিসিভার: মূল মান সরাসরি পরিবর্তন করে
func (c *Counter) IncrementPtr() {
    c.Count++
}

func main() {
    c := Counter{Count: 0}

    c.IncrementVal()
    fmt.Println("IncrementVal এর পর:", c.Count)

    c.IncrementPtr()
    fmt.Println("IncrementPtr এর পর:", c.Count)
}
```

আউটপুট:

```text
IncrementVal এর পর: 0
IncrementPtr এর পর: 1
```

> **টিপ:** যদি কোনো মেথডের মাধ্যমে স্ট্রাক্টের মান পরিবর্তন করতে চান, অথবা স্ট্রাক্টটি সাইজে বড় হয়—তাহলে পয়েন্টার রিসিভার ব্যবহার করাই গো-এর স্ট্যান্ডার্ড কনভেনশন।

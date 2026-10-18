# আধুনিক ইটারেটর (Iterators)

গো ১.২৩ থেকে শুরু করে আধুনিক গো-তে যুক্ত হয়েছে অফিশিয়াল ইটারেটর ও রেঞ্জ-ওভার-ফাংশন (Range over Function) সুবিধা। এর ফলে কাস্টম ডেটা স্ট্রাকচার, ফিল্টার বা জেনারেটরের জন্য নিজস্ব লুপ লজিক লেখা যায় এবং সরাসরি স্ট্যান্ডার্ড `for ... range` দিয়ে সেগুলোর উপর লুপ চালানো যায়।

---

## ১. iter.Seq এবং কাস্টম ইটারেটর তৈরি

গো-এর স্ট্যান্ডার্ড `iter` প্যাকেজে দুটি মূল টাইপ রয়েছে:
- `iter.Seq[V]`: একক ভ্যালু প্রদানকারী ইটারেটর ফাংশন `func(yield func(V) bool)`
- `iter.Seq2[K, V]`: কি-ভ্যালু বা জোড়া ভ্যালু প্রদানকারী ইটারেটর ফাংশন `func(yield func(K, V) bool)`

এখানে `yield` ফাংশনটি প্রতিবার লুপের বডিকে একটি নতুন ভ্যালু সরবরাহ করে। যদি লুপের ভেতর `break` কল করা হয়, তবে `yield` রিটার্ন করে `false` এবং ইটারেশন থেমে যায়।

```go
package main

import (
    "fmt"
    "iter"
)

// জোড় সংখ্যা উৎপন্ন করার কাস্টম ইটারেটর
func Evens(limit int) iter.Seq[int] {
    return func(yield func(int) bool) {
        for i := 2; i <= limit; i += 2 {
            if !yield(i) {
                return // লুপ ব্রেক হলে থেমে যাবে
            }
        }
    }
}

func main() {
    // সরাসরি কাস্টম ইটারেটরের উপর for ... range চালানো
    for num := range Evens(10) {
        fmt.Println("Even:", num)
    }
}
```

আউটপুট:

```text
Even: 2
Even: 4
Even: 6
Even: 8
Even: 10
```

---

## ২. iter.Seq2 এবং কি-ভ্যালু জোড়া

দুটি ভ্যালুর (যেমন ইনডেক্স এবং মান, অথবা কি এবং ভ্যালু) জন্য `iter.Seq2` ব্যবহার করা হয়:

```go
package main

import (
    "fmt"
    "iter"
)

// Fibonacci অনুক্রমের ইনডেক্স ও মান প্রদানকারী ইটারেটর
func Fib(n int) iter.Seq2[int, int] {
    return func(yield func(int, int) bool) {
        a, b := 0, 1
        for i := 0; i < n; i++ {
            if !yield(i, a) {
                return
            }
            a, b = b, a+b
        }
    }
}

func main() {
    for idx, val := range Fib(6) {
        fmt.Printf("Fibonacci #%d: %d\n", idx, val)
    }
}
```

আউটপুট:

```text
Fibonacci #0: 0
Fibonacci #1: 1
Fibonacci #2: 1
Fibonacci #3: 2
Fibonacci #4: 3
Fibonacci #5: 5
```

---

## ৩. slices.Collect এবং স্ট্যান্ডার্ড লাইব্রেরি সমন্বয়

একটি ইটারেটর থেকে সরাসরি নতুন স্লাইস তৈরি করতে আধুনিক `slices.Collect` ব্যবহার করা যায়:

```go
package main

import (
    "fmt"
    "iter"
    "slices"
)

func CountTo(n int) iter.Seq[int] {
    return func(yield func(int) bool) {
        for i := 1; i <= n; i++ {
            if !yield(i) {
                return
            }
        }
    }
}

func main() {
    // ইটারেটরকে সরাসরি একটি স্লাইসে রূপান্তর
    numbers := slices.Collect(CountTo(5))
    fmt.Println("সংগ্রহ করা স্লাইস:", numbers)
}
```

আউটপুট:

```text
সংগ্রহ করা স্লাইস: [1 2 3 4 5]
```

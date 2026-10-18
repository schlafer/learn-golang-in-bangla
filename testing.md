# টেস্টিং ও বেঞ্চমার্কিং (Testing & Benchmarking)

গো প্রোগ্রামিংয়ে স্বয়ংক্রিয় টেস্টিং কোনো থার্ড-পার্টি প্লাগইন বা ফ্রেমওয়ার্কের উপর নির্ভরশীল নয়—গো-এর মূল টুলচেইনের সাথে প্রথম থেকেই বিল্টইন `testing` প্যাকেজ এবং `go test` কমান্ড সরবরাহ করা থাকে।

---

## ১. ইউনিট টেস্টের নিয়মাবলি

গো-তে টেস্ট লেখার ৩টি মূল নিয়ম:
1. টেস্ট ফাইলের নাম অবশ্যই `_test.go` দিয়ে শেষ হতে হবে (যেমন `math_test.go`)।
2. টেস্ট ফাংশনের নাম অবশ্যই `Test` দিয়ে শুরু হতে হবে এবং প্রথম বর্ণ বড় হাতের হতে হবে (যেমন `TestAdd`)।
3. ফাংশনের একমাত্র প্যারামিটার হবে `t *testing.T`।

```go
package main

import "testing"

// মূল ফাংশন
func Add(a, b int) int {
    return a + b
}

// ইউনিট টেস্ট
func TestAdd(t *testing.T) {
    got := Add(2, 3)
    want := 5

    if got != want {
        t.Errorf("Add(2, 3) = %d; প্রত্যাশিত মান %d", got, want)
    }
}
```

আউটপুট:

```text
=== RUN   TestAdd
--- PASS: TestAdd (0.00s)
PASS
```

---

## ২. টেবিল-ড্রিভেন টেস্ট (Table-Driven Tests)

গো কমিউনিটিতে টেস্ট লেখার সবচেয়ে জনপ্রিয় ও আদর্শ প্যাটার্ন হলো টেবিল-ড্রিভেন টেস্ট। একাধিক টেস্ট কেসকে একটি স্ট্রাক্ট স্লাইসে রেখে লুপের মাধ্যমে `t.Run` দিয়ে সাব-টেস্ট আকারে চালানো হয়:

```go
package main

import "testing"

func IsEven(n int) bool {
    return n%2 == 0
}

func TestIsEven(t *testing.T) {
    // টেস্ট কেসগুলোর টেবিল
    tests := []struct {
        name string
        num  int
        want bool
    }{
        {"ধনাত্মক জোড়", 4, true},
        {"ধনাত্মক বিজোড়", 7, false},
        {"শূন্য", 0, true},
        {"ঋণাত্মক বিজোড়", -3, false},
    }

    for _, tt := range tests {
        t.Run(tt.name, func(t *testing.T) {
            got := IsEven(tt.num)
            if got != tt.want {
                t.Errorf("IsEven(%d) = %v; প্রত্যাশিত %v", tt.num, got, tt.want)
            }
        })
    }
}
```

আউটপুট:

```text
=== RUN   TestIsEven
=== RUN   TestIsEven/ধনাত্মক_জোড়
=== RUN   TestIsEven/ধনাত্মক_বিজোড়
=== RUN   TestIsEven/শূন্য
=== RUN   TestIsEven/ঋণাত্মক_বিজোড়
--- PASS: TestIsEven (0.00s)
    --- PASS: TestIsEven/ধনাত্মক_জোড় (0.00s)
    --- PASS: TestIsEven/ধনাত্মক_বিজোড় (0.00s)
    --- PASS: TestIsEven/শূন্য (0.00s)
    --- PASS: TestIsEven/ঋণাত্মক_বিজোড় (0.00s)
PASS
```

---

## ৩. বেঞ্চমার্কিং (Benchmark)

কোডের গতি এবং পারফরম্যান্স পরিমাপের জন্য গো-তে রয়েছে বিল্টইন বেঞ্চমার্ক সুবিধা। বেঞ্চমার্ক ফাংশন `Benchmark` দিয়ে শুরু হয় এবং `b *testing.B` গ্রহণ করে:

```go
package main

import (
    "strings"
    "testing"
)

func BenchmarkStringConcat(b *testing.B) {
    // b.N হলো রানটাইম নির্ধারিত ইটারেশন সংখ্যা
    for i := 0; i < b.N; i++ {
        var sb strings.Builder
        sb.WriteString("hello")
        sb.WriteString("world")
        _ = sb.String()
    }
}
```

আউটপুট:

```text
BenchmarkStringConcat-8    50000000    24.5 ns/op
PASS
```

টার্মিনালে টেস্ট ও বেঞ্চমার্ক চালানোর দরকারি কমান্ডসমূহ:
- সমস্ত টেস্ট বিস্তারিত দেখা: `go test -v ./...`
- টেস্ট কভারেজ দেখা: `go test -cover`
- বেঞ্চমার্ক রান করা: `go test -bench=.`

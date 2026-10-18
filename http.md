# বেসিক এইচটিটিপি ক্লায়েন্ট ও সার্ভার (net/http)

গো প্রোগ্রামিং ল্যাঙ্গুয়েজের সবচেয়ে জনপ্রিয় এবং শক্তিশালী বৈশিষ্ট্যগুলোর একটি হলো এর বিল্টইন `net/http` প্যাকেজ। কোনো থার্ড-পার্টি ফ্রেমওয়ার্ক ছাড়া শুধুমাত্র গো-এর স্ট্যান্ডার্ড লাইব্রেরি ব্যবহার করেই উচ্চ ক্ষমতাসম্পন্ন ওয়েব সার্ভার ও এপিআই তৈরি করা সম্ভব।

---

## ১. এইচটিটিপি হ্যান্ডলার এবং রিকোয়েস্ট টেস্ট করা

গো-তে একটি এইচটিটিপি হ্যান্ডলার ফাংশন দুটি আর্গুমেন্ট গ্রহণ করে:
- `http.ResponseWriter`: যার মাধ্যমে ক্লায়েন্টকে রেসপন্স (HTML, JSON, বা Text) পাঠানো হয়।
- `*http.Request`: যা ক্লায়েন্টের পাঠানো রিকোয়েস্টের তথ্য (URL, Headers, Method, Body) ধারণ করে।

নিচের প্রোগ্রামে একটি এইচটিটিপি সার্ভার তৈরি করে `http.Get` দিয়ে সরাসরি রিকোয়েস্ট পাঠিয়ে রেসপন্স পাওয়ার সম্পূর্ণ প্রক্রিয়াটি দেখানো হয়েছে:

```go
package main

import (
    "fmt"
    "io"
    "net/http"
    "net/http/httptest"
)

// আমাদের হ্যান্ডলার ফাংশন
func helloHandler(w http.ResponseWriter, r *http.Request) {
    fmt.Fprintln(w, "হ্যালো, এটি গো এইচটিটিপি সার্ভার থেকে রেসপন্স!")
}

func main() {
    // টেস্ট সার্ভার চালু করা
    server := httptest.NewServer(http.HandlerFunc(helloHandler))
    defer server.Close()

    // http.Get দিয়ে সার্ভারে রিকোয়েস্ট পাঠানো
    resp, err := http.Get(server.URL)
    if err != nil {
        fmt.Println("রিকোয়েস্ট এরর:", err)
        return
    }
    defer resp.Body.Close()

    // রেসপন্স বডি পড়া
    body, _ := io.ReadAll(resp.Body)
    fmt.Println("স্ট্যাটাস কোড:", resp.StatusCode)
    fmt.Print("রেসপন্স বডি: ", string(body))
}
```

আউটপুট:

```text
স্ট্যাটাস কোড: 200
রেসপন্স বডি: হ্যালো, এটি গো এইচটিটিপি সার্ভার থেকে রেসপন্স!
```

---

## ২. লোকাল মেশিনে স্ট্যান্ডঅ্যালোন ওয়েব সার্ভার চালানো

আপনার কম্পিউটারের কোনো নির্দিষ্ট পোর্টে (যেমন পোর্ট ৮০৮০) একটি পূর্ণাঙ্গ সার্ভার চালু করে ব্রাউজারে দেখতে চাইলে নিচের মতো `http.ListenAndServe` ব্যবহার করতে হয়:

```go
package main

import (
    "fmt"
    "net/http"
)

func main() {
    // রুট পাথে হ্যান্ডলার যোগ করা
    http.HandleFunc("/", func(w http.ResponseWriter, r *http.Request) {
        fmt.Fprintf(w, "স্বাগতম! আপনার রিকোয়েস্ট পাথ: %s", r.URL.Path)
    })

    fmt.Println("সার্ভার চালু হচ্ছে http://localhost:8080 এ...")
    // বাস্তব প্রজেক্টে টার্মিনালে এটি রান করে ব্রাউজারে localhost:8080 ওপেন করতে হয়
    // http.ListenAndServe(":8080", nil)
}
```

আউটপুট:

```text
সার্ভার চালু হচ্ছে http://localhost:8080 এ...
```

টার্মিনালে `go run .` দিয়ে সার্ভার চালু করার পর ব্রাউজারে `http://localhost:8080` ভিজিট করলেই আপনার তৈরি সার্ভারের রেসপন্স দেখতে পাবেন!

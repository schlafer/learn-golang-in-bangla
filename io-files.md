# ফাইল ও আই/ও (I/O)

প্রোগ্রামিংয়ে ডিস্কে ডেটা সংরক্ষণ (File Writing) এবং ফাইল থেকে ডেটা পড়া (File Reading) অত্যন্ত মৌলিক কাজ। গো-তে `os`, `io`, এবং `bufio` প্যাকেজ অত্যন্ত শক্তিশালী এবং সহজবোধ্য ফাইল আই/ও অপারেশন সুবিধা দেয়।

---

## ১. দ্রুত ফাইল তৈরি ও পড়া (`os.WriteFile` ও `os.ReadFile`)

সম্পূর্ণ ফাইলের কন্টেন্ট একবারে লিখতে বা পড়তে সবচেয়ে সহজ হলো `os.WriteFile` এবং `os.ReadFile`:

```go
package main

import (
    "fmt"
    "os"
)

func main() {
    filePath := "test_sample.txt"
    content := []byte("গো ল্যাঙ্গুয়েজ দিয়ে ফাইল অপারেশন শেখা।\nসহজ ও দ্রুত।")

    // ফাইল তৈরি এবং লেখা (0644 হলো স্ট্যান্ডার্ড ফাইল পারমিশন)
    err := os.WriteFile(filePath, content, 0644)
    if err != nil {
        fmt.Println("ফাইল লিখতে সমস্যা:", err)
        return
    }

    // ফাইল থেকে সম্পূর্ণ ডেটা পড়া
    data, err := os.ReadFile(filePath)
    if err != nil {
        fmt.Println("ফাইল পড়তে সমস্যা:", err)
        return
    }

    fmt.Println("ফাইল থেকে পড়া কন্টেন্ট:\n" + string(data))

    // ক্লিনআপ: ফাইল মুছে ফেলা
    _ = os.Remove(filePath)
}
```

আউটপুট:

```text
ফাইল থেকে পড়া কন্টেন্ট:
গো ল্যাঙ্গুয়েজ দিয়ে ফাইল অপারেশন শেখা।
সহজ ও দ্রুত।
```

---

## ২. বড় ফাইল লাইন বাই লাইন পড়া (`bufio.Scanner`)

ফাইল যদি অনেক বড় হয় (যেমন মেগাবাইট বা গিগাবাইট সাইজের লগ ফাইল), তখন সম্পূর্ণ ফাইল একবারে মেমোরিতে লোড না করে `bufio.Scanner` দিয়ে লাইন বাই লাইন পড়া নিরাপদ:

```go
package main

import (
    "bufio"
    "fmt"
    "strings"
)

func main() {
    // ডেমোর জন্য একটি মাল্টি-লাইন ইনপুট রিডার তৈরি করলাম
    sampleText := "প্রথম লাইন\nদ্বিতীয় লাইন\nতৃতীয় লাইন"
    scanner := bufio.NewScanner(strings.NewReader(sampleText))

    lineCount := 1
    for scanner.Scan() {
        fmt.Printf("লাইন %d: %s\n", lineCount, scanner.Text())
        lineCount++
    }

    if err := scanner.Err(); err != nil {
        fmt.Println("স্ক্যানিং এরর:", err)
    }
}
```

আউটপুট:

```text
লাইন 1: প্রথম লাইন
লাইন 2: দ্বিতীয় লাইন
লাইন 3: তৃতীয় লাইন
```

---

## ৩. io.Reader এবং io.Writer ইন্টারফেস

গো-এর পুরো আই/ও ইকোসিস্টেম গড়ে উঠেছে দুটি অতি সাধারণ ইন্টারফেসের উপর:
- `io.Reader`: `Read(p []byte) (n int, err error)`
- `io.Writer`: `Write(p []byte) (n int, err error)`

ফাইল, নেটওয়ার্ক সকেট, এইচটিটিপি রিকোয়েস্ট বডি, বাফার—সবকিছুই এই ইন্টারফেসগুলো বাস্তবায়ন করে। ফলে যেকোনো সোর্স থেকে যেকোনো ডেস্টিনেশনে ডেটা স্থানান্তর করতে `io.Copy(writer, reader)` ব্যবহার করা যায়।

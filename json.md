# জেসন ও ডাটা সিরিয়ালাইজেশন (JSON)

ওয়েব ডেভেলপমেন্ট ও এপিআই (API) তৈরিতে জেসন (JSON - JavaScript Object Notation) বহুল ব্যবহৃত ডেটা ফরম্যাট। গো-এর স্ট্যান্ডার্ড লাইব্রেরির `encoding/json` প্যাকেজের মাধ্যমে অত্যন্ত সহজে জেসন এনকোডিং ও ডিকোডিং করা যায়।

---

## ১. স্ট্রাক্টকে জেসনে রূপান্তর (Marshal) এবং স্ট্রাক্ট ট্যাগ

গো অবজেক্টকে জেসন স্ট্রিংয়ে রূপান্তর করতে `json.Marshal` ব্যবহার করা হয়। স্ট্রাক্টের ফিল্ড জেসনে কী নামে যাবে তা নিয়ন্ত্রণ করতে স্ট্রাক্ট ট্যাগ (Struct Tags) ব্যবহৃত হয়:

- `json:"name"`: জেসন কি-এর নাম নির্ধারণ করে।
- `json:"email,omitempty"`: মান ফাঁকা হলে জেসন থেকে ফিল্ডটি বাদ দেয়।
- `json:"-"`: ফিল্ডটিকে জেসন আউটপুট থেকে সম্পূর্ণ উপেক্ষা করে।

```go
package main

import (
    "encoding/json"
    "fmt"
)

type User struct {
    ID       int    `json:"id"`
    FullName string `json:"full_name"`
    Email    string `json:"email,omitempty"`
    Password string `json:"-"` // গোপন তথ্য জেসনে যাবে না
}

func main() {
    u := User{
        ID:       101,
        FullName: "তানভীর আহমেদ",
        Email:    "tanvir@example.com",
        Password: "secret-password",
    }

    // সুন্দরভাবে ইনডেন্ট সহ জেসন তৈরি
    jsonData, err := json.MarshalIndent(u, "", "  ")
    if err != nil {
        fmt.Println("মার্শালিং এরর:", err)
        return
    }

    fmt.Println(string(jsonData))
}
```

আউটপুট:

```text
{
  "id": 101,
  "full_name": "তানভীর আহমেদ",
  "email": "tanvir@example.com"
}
```

---

## ২. জেসন থেকে গো অবজেক্টে রূপান্তর (Unmarshal)

বাইটের আকারে থাকা জেসন ডেটাকে গো স্ট্রাক্টে ডিকোড করতে `json.Unmarshal` ব্যবহার করা হয় (পয়েন্টার পাস করতে হয়):

```go
package main

import (
    "encoding/json"
    "fmt"
)

type Product struct {
    Title string  `json:"title"`
    Price float64 `json:"price"`
}

func main() {
    rawJSON := `{"title": "গো প্রোগ্রামিং বই", "price": 450.50}`

    var p Product
    err := json.Unmarshal([]byte(rawJSON), &p)
    if err != nil {
        fmt.Println("আনমার্শাল এরর:", err)
        return
    }

    fmt.Println("বইয়ের নাম:", p.Title)
    fmt.Printf("মূল্য: %.2f টাকা\n", p.Price)
}
```

আউটপুট:

```text
বইয়ের নাম: গো প্রোগ্রামিং বই
মূল্য: 450.50 টাকা
```

---

## ৩. ডাইনামিক জেসন (map[string]any)

আগে থেকে স্ট্রাক্ট জানা না থাকলে `map[string]any` ব্যবহার করে যেকোনো রূপহীন জেসন পার্স করা সম্ভব:

```go
package main

import (
    "encoding/json"
    "fmt"
)

func main() {
    rawJSON := `{"status": "success", "code": 200}`

    var result map[string]any
    _ = json.Unmarshal([]byte(rawJSON), &result)

    fmt.Println("স্ট্যাটাস:", result["status"])
    fmt.Println("কোড:", result["code"])
}
```

আউটপুট:

```text
স্ট্যাটাস: success
কোড: 200
```

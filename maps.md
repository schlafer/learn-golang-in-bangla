# ম্যাপস

গো-তে ম্যাপ (Map) হলো অ্যাসোসিয়েটিভ ডাটা স্ট্রাকচার (যা অন্যান্য ল্যাঙ্গুয়েজে হ্যাশম্যাপ বা ডিকশনারি নামে পরিচিত)। এতে ডাটা কি-ভ্যালু (key-value pair) আকারে সংরক্ষিত থাকে।

---

## ১. ম্যাপ তৈরি এবং মৌলিক ব্যবহার

আমরা বিল্টইন `make` ফাংশন ব্যবহার করে ম্যাপ তৈরি করতে পারি:

```go
package main

import "fmt"

func main() {
    // string কি এবং int ভ্যালুর ম্যাপ তৈরি
    m := make(map[string]int)

    // কি-ভ্যালু অ্যাসাইন করা
    m["apple"] = 5
    m["banana"] = 12

    fmt.Println("সম্পূর্ণ ম্যাপ:", m)
    fmt.Println("apple এর মান:", m["apple"])
    fmt.Println("ম্যাপের দৈর্ঘ্য (len):", len(m))

    // উপাদান মুছে ফেলা (delete)
    delete(m, "apple")
    fmt.Println("delete করার পর:", m)
}
```

আউটপুট:

```text
সম্পূর্ণ ম্যাপ: map[apple:5 banana:12]
apple এর মান: 5
ম্যাপের দৈর্ঘ্য (len): 2
delete করার পর: map[banana:12]
```

---

## ২. কি উপস্থিত আছে কিনা পরীক্ষা করা (Comma-ok idiom)

গো-তে কোনো কি ম্যাপে আদৌ আছে নাকি সেটি তার টাইপের জিরো ভ্যালু, তা যাচাই করতে ২-ভ্যালু অ্যাসাইনমেন্ট (comma-ok idiom) ব্যবহার করা হয়:

```go
package main

import "fmt"

func main() {
    scores := map[string]int{
        "alice": 95,
        "bob":   0, // ববের স্কোর ০
    }

    // চার্লি ম্যাপে নেই
    score, exists := scores["charlie"]
    fmt.Println("Charlie score:", score, "Exists?:", exists)

    // বব ম্যাপে আছে এবং স্কোর ০
    score, exists = scores["bob"]
    fmt.Println("Bob score:", score, "Exists?:", exists)
}
```

আউটপুট:

```text
Charlie score: 0 Exists?: false
Bob score: 0 Exists?: true
```

---

## ৩. আধুনিক ফিচার: clear() এবং maps প্যাকেজ

আধুনিক গো-তে ম্যাপ নিয়ে কাজের জন্য বিল্টইন `clear()` এবং স্ট্যান্ডার্ড `maps` প্যাকেজ রয়েছে:

```go
package main

import (
    "fmt"
    "maps"
)

func main() {
    m1 := map[string]int{"a": 1, "b": 2}

    // ম্যাপ ক্লোন করা (Clone)
    m2 := maps.Clone(m1)
    fmt.Println("ক্লোন করা ম্যাপ:", m2)

    // দুটি ম্যাপ সমান কিনা যাচাই (Equal)
    fmt.Println("ম্যাপ দুটি সমান?:", maps.Equal(m1, m2))

    // সব উপাদান খালি করা (clear)
    clear(m1)
    fmt.Println("clear করার পর m1:", m1)
}
```

আউটপুট:

```text
ক্লোন করা ম্যাপ: map[a:1 b:2]
ম্যাপ দুটি সমান?: true
clear করার পর m1: map[]
```

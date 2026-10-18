# রেঞ্জ

গো-তে স্লাইস, এ্যারে, ম্যাপ, স্ট্রিং কিংবা নির্দিষ্ট সংখ্যার উপর পুনরাবৃত্তি বা লুপ করার জন্য `for ... range` কনস্ট্রাক্ট ব্যবহৃত হয়।

---

## ১. স্লাইস ও এ্যারের উপর রেঞ্জ

স্লাইসের উপর `range` চালালে এটি প্রতিটি উপাদানের জন্য দুটি মান দেয়: ইনডেক্স এবং ওই ইনডেক্সের মান:

```go
package main

import "fmt"

func main() {
    numbers := []int{10, 20, 30}

    // ইনডেক্স ও ভ্যালু উভয়ই ব্যবহার করা
    for i, v := range numbers {
        fmt.Println("Index:", i, "Value:", v)
    }

    // ইনডেক্স প্রয়োজন না হলে ব্ল্যাঙ্ক আইডেন্টিফায়ার (_) দিয়ে বাদ দেওয়া যায়
    sum := 0
    for _, num := range numbers {
        sum += num
    }
    fmt.Println("যোগফল:", sum)
}
```

আউটপুট:

```text
Index: 0 Value: 10
Index: 1 Value: 20
Index: 2 Value: 30
যোগফল: 60
```

---

## ২. ম্যাপের উপর রেঞ্জ

ম্যাপের প্রতিটি উপাদান ভিজিট করার সময় `range` দেয় কি (key) এবং ভ্যালু (value):

```go
package main

import "fmt"

func main() {
    capitals := map[string]string{
        "Bangladesh": "Dhaka",
        "Japan":      "Tokyo",
    }

    for country, city := range capitals {
        fmt.Printf("%s এর রাজধানী %s\n", country, city)
    }
}
```

আউটপুট:

```text
Bangladesh এর রাজধানী Dhaka
Japan এর রাজধানী Tokyo
```

---

## ৩. স্ট্রিং এবং ইউনিকোড (Rune) এর উপর রেঞ্জ

স্ট্রিং-এর উপর `range` চালালে এটি বাইট ইনডেক্স এবং ইউনিকোড ক্যারেক্টার বা `rune` রিটার্ন করে:

```go
package main

import "fmt"

func main() {
    for i, ch := range "গো" {
        fmt.Printf("Index %d: %c\n", i, ch)
    }
}
```

আউটপুট:

```text
Index 0: গ
Index 3: ো
```

---

## ৪. আধুনিক ইন্টিজার রেঞ্জ (Go 1.22+)

শুধুমাত্র একটি সংখ্যার উপর রেঞ্জ চালিয়ে ০ থেকে শুরু করে নির্দিষ্ট সংখ্যক বার লুপ ঘোরানো যায়:

```go
package main

import "fmt"

func main() {
    for i := range 3 {
        fmt.Println("Round:", i)
    }
}
```

আউটপুট:

```text
Round: 0
Round: 1
Round: 2
```

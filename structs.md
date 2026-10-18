# স্ট্রাক্ট ও কাস্টম টাইপ

গো-তে স্ট্রাক্ট (Struct) হলো বিভিন্ন টাইপের ফিল্ডের সমষ্টি দিয়ে তৈরি একটি ইউজার-ডিফাইন্ড কাস্টম ডেটা টাইপ। অবজেক্ট ওরিয়েন্টেড প্রোগ্রামিংয়ের "ক্লাস" বা রেকর্ডের ধারণাকে গো-তে স্ট্রাক্টের মাধ্যমে প্রকাশ করা হয়।

---

## ১. স্ট্রাক্ট ডিক্লেয়ার ও ইনিশিয়ালাইজ করা

স্ট্রাক্ট ডিফাইন করতে `type` এবং `struct` কি-ওয়ার্ড ব্যবহার করা হয়:

```go
package main

import "fmt"

// Person নামের একটি কাস্টম স্ট্রাক্ট
type Person struct {
    Name string
    Age  int
}

func main() {
    // ফিল্ডের নাম উল্লেখ করে ইনিশিয়ালাইজ
    p1 := Person{Name: "রহিম", Age: 25}
    fmt.Println("ব্যক্তি ১:", p1)

    // ফিল্ডের নাম ছাড়া ক্রমানুসারে
    p2 := Person{"করিম", 30}
    fmt.Println("ব্যক্তি ২:", p2)

    // ডট (.) দিয়ে ফিল্ড এক্সেস ও পরিবর্তন
    fmt.Println("p1 এর নাম:", p1.Name)
    p1.Age = 26
    fmt.Println("p1 এর পরিবর্তিত বয়স:", p1.Age)
}
```

আউটপুট:

```text
ব্যক্তি ১: {রহিম 25}
ব্যক্তি ২: {করিম 30}
p1 এর নাম: রহিম
p1 এর পরিবর্তিত বয়স: 26
```

---

## ২. স্ট্রাক্টের পয়েন্টার (Pointers to Structs)

স্ট্রাক্ট যখন বড় হয়, তখন সেটি কপি না করে পয়েন্টারের মাধ্যমে পাস করা বেশি সাশ্রয়ী ও দ্রুত। গো-তে স্ট্রাক্ট পয়েন্টারের ক্ষেত্রে সরাসরি ডট দিয়ে ফিল্ড এক্সেস করা যায় (আলাদা করে `(*ptr).Field` লিখতে হয় না):

```go
package main

import "fmt"

type ServerConfig struct {
    Host string
    Port int
}

func main() {
    cfg := &ServerConfig{Host: "localhost", Port: 8080}

    // স্বয়ংক্রিয় ডিরিফারেন্স
    fmt.Println("Host:", cfg.Host)
    cfg.Port = 9000
    fmt.Println("Updated Port:", cfg.Port)
}
```

আউটপুট:

```text
Host: localhost
Updated Port: 9000
```

---

## ৩. স্ট্রাক্ট এম্বেডিং (Composition / কম্পোজিশন)

গো-তে ইনহেরিট্যান্স (inheritance) নেই। এর বদলে গো উৎসাহিত করে "কম্পোজিশন" বা এম্বেডিং। একটি স্ট্রাক্টের ভেতর অন্য স্ট্রাক্ট সরাসরি এম্বেড করে সমস্ত ফিল্ড নিজের মতো ব্যবহার করা যায়:

```go
package main

import "fmt"

type Address struct {
    City    string
    Country string
}

type User struct {
    Name    string
    Address // এম্বেডেড স্ট্রাক্ট
}

func main() {
    u := User{
        Name: "আরিফ",
        Address: Address{
            City:    "ঢাকা",
            Country: "বাংলাদেশ",
        },
    }

    // সরাসরি এম্বেডেড ফিল্ড এক্সেস করা যায়
    fmt.Println("ব্যবহারকারী:", u.Name)
    fmt.Println("শহর:", u.City)
    fmt.Println("দেশ:", u.Country)
}
```

আউটপুট:

```text
ব্যবহারকারী: আরিফ
শহর: ঢাকা
দেশ: বাংলাদেশ
```

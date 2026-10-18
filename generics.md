# জেনেরিকস

গো ১.১৮ সংস্করণে যোগ হওয়া অন্যতম প্রধান এবং যুগান্তকারী বৈশিষ্ট্য হলো জেনেরিকস (Generics / Parametric Polymorphism)। এর মাধ্যমে টাইপ-সেফটি অক্ষুণ্ণ রেখে যেকোনো টাইপের জন্য এক কোড পুনরায় ব্যবহার করা যায়—বারবার কোড ডুপ্লিকেশন বা `any` ব্যবহার করে রানটাইম টাইপ অ্যাসারশনের প্রয়োজন হয় না।

---

## ১. জেনেরিক ফাংশন এবং টাইপ প্যারামিটার

ফাংশন নামের পর থার্ড ব্র্যাকেটে `[T any]` দিয়ে টাইপ প্যারামিটার নির্দিষ্ট করা হয়:

```go
package main

import "fmt"

// যেকোনো টাইপের স্লাইস রিভার্স বা উল্টানোর জেনেরিক ফাংশন
func Reverse[T any](s []T) []T {
    result := make([]T, len(s))
    for i, v := range s {
        result[len(s)-1-i] = v
    }
    return result
}

func main() {
    intList := []int{1, 2, 3, 4, 5}
    reversedInts := Reverse(intList)
    fmt.Println("উল্টানো সংখ্যা:", reversedInts)

    strList := []string{"গো", "পাইথন", "রাস্ট"}
    reversedStrs := Reverse(strList)
    fmt.Println("উল্টানো স্ট্রিং:", reversedStrs)
}
```

আউটপুট:

```text
উল্টানো সংখ্যা: [5 4 3 2 1]
উল্টানো স্ট্রিং: [রাস্ট পাইথন গো]
```

---

## ২. টাইপ কনস্ট্রেইন্ট এবং comparable

সব টাইপের উপর সব অপারেশন করা যায় না (যেমন তুলনা করা বা যোগ করা)। নির্দিষ্ট টাইপ সীমাবদ্ধ করতে কনস্ট্রেইন্ট (Type Constraints) ব্যবহার করা হয়:

- `any`: যেকোনো টাইপ অনুমোদিত।
- `comparable`: যেসকল টাইপ `==` এবং `!=` দিয়ে তুলনা করা যায় (যেমন int, string, pointer)।
- কাস্টম ইউনিয়ন: যেমন `int | float64`।

```go
package main

import "fmt"

// Number কনস্ট্রেইন্ট যা int অথবা float64 সমর্থন করে
type Number interface {
    int | float64
}

func Add[T Number](a, b T) T {
    return a + b
}

// comparable কনস্ট্রেইন্ট ব্যবহার করে স্লাইসে উপাদান খোঁজা
func IndexOf[T comparable](s []T, target T) int {
    for i, v := range s {
        if v == target {
            return i
        }
    }
    return -1
}

func main() {
    fmt.Println("পূর্ণসংখ্যার যোগফল:", Add(10, 20))
    fmt.Println("দশমিক সংখ্যার যোগফল:", Add(3.14, 2.86))

    names := []string{"রহিম", "করিম", "আরিফ"}
    fmt.Println("করিমের ইনডেক্স:", IndexOf(names, "করিম"))
}
```

আউটপুট:

```text
পূর্ণসংখ্যার যোগফল: 30
দশমিক সংখ্যার যোগফল: 6
করিমের ইনডেক্স: 1
```

---

## ৩. জেনেরিক স্ট্রাক্ট (Generic Data Structures)

ফাংশনের পাশাপাশি স্ট্রাক্টেও জেনেরিক টাইপ ব্যবহার করা যায়:

```go
package main

import "fmt"

// যেকোনো টাইপের জন্য একটি জেনেরিক স্ট্যাক (Stack)
type Stack[T any] struct {
    elements []T
}

func (s *Stack[T]) Push(item T) {
    s.elements = append(s.elements, item)
}

func (s *Stack[T]) Pop() (T, bool) {
    if len(s.elements) == 0 {
        var zero T
        return zero, false
    }
    lastIndex := len(s.elements) - 1
    item := s.elements[lastIndex]
    s.elements = s.elements[:lastIndex]
    return item, true
}

func main() {
    stack := Stack[string]{}
    stack.Push("প্রথম")
    stack.Push("দ্বিতীয়")

    val, _ := stack.Pop()
    fmt.Println("পপ করা মান:", val)
}
```

আউটপুট:

```text
পপ করা মান: দ্বিতীয়
```

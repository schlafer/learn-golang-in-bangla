# বাংলায় গো প্রোগ্রামিং (Learn Go in Bangla)

আধুনিক গো (Go 1.27+) প্রোগ্রামিং ভাষার সহজবোধ্য ও বাস্তবমুখী গাইডবুক।

---

## পরিচিতি

Go (যা সাধারণত Golang নামে পরিচিত) ২০০৭ সালে গুগলে রবার্ট গ্রিজমার (Robert Griesemer), রব পাইক (Rob Pike) এবং কেন থম্পসন (Ken Thompson) দ্বারা তৈরি করা হয় এবং ২০০৯ সালে এটি ওপেন-সোর্স হিসেবে উন্মুক্ত করা হয়। আধুনিক ক্লাউড কম্পিউটিং, মাইক্রোসার্ভিস, ডিস্ট্রিবিউটেড সিস্টেম এবং হাই-পারফর্ম্যান্স ব্যাকএন্ড সার্ভার তৈরিতে গো বিশ্বজুড়ে অত্যন্ত জনপ্রিয়।

এই বইটি মূলত [`howtocode-dev/golang.howtocode.dev`](https://github.com/howtocode-dev/golang.howtocode.dev) ওপেন-সোর্স উদ্যোগের একটি আধুনিক ও সক্রিয় সংস্করণ (fork)। মূল প্রজেক্টটি দীর্ঘদিন ধরে রক্ষণাবেক্ষণ না হওয়ায়, গো-এর সাম্প্রতিক সংস্করণ **Go 1.27.1**-এর সাথে সামঞ্জস্য রেখে পুরো বইটি নতুন করে সাজানো ও সম্প্রসারিত করা হয়েছে।

## এই সংস্করণের বিশেষত্ব

- **আধুনিক গো ১.২৭+ মানদণ্ড:** গো-এর সাম্প্রতিক সিনট্যাক্স ও ফিচারসমূহ—যেমন `for i := range n` লুপ, `slices` ও `maps` স্ট্যান্ডার্ড লাইব্রেরি প্যাকেজ, আধুনিক স্ট্রাকচার্ড লগিং (`log/slog`), জেনেরিকস এবং আইটারেটর সমর্থন।
- **বাস্তবমুখী ও সহজবোধ্য বাংলা:** অপ্রয়োজনীয় ও জটিল আক্ষরিক অনুবাদ পরিহার করে প্রোগ্রামারদের স্বাভাবিক ও ব্যবহারোপযোগী বাংলা ভাষায় রচিত।
- **৩১টি বিস্তারিত অধ্যায়:** মৌলিক সিনট্যাক্স থেকে শুরু করে কনকারেন্সি (`goroutine`, `channel`, `select`, `sync.Mutex`), ফাইল আই/ও, এইচটিটিপি ক্লায়েন্ট ও সার্ভার, কনটেক্সট এবং অটোমেটেড টেস্টিং।
- **যাচাইকৃত কোড উদাহরণ:** বইয়ের প্রতিটি কোড উদাহরণ স্বয়ংক্রিয় টেস্ট সুইট দিয়ে কম্পাইল ও রান করে পরীক্ষিত।

---

## অধ্যায়সূচি (Table of Contents)

### শুরু ও মৌলিক সিনট্যাক্স
- [শুরুর আগে](intro.md)
- [হ্যালো গো](hello-go.md)
- [ভ্যালুজ](values.md)
- [ভ্যারিয়েবলস](variables.md)
- [কন্সট্যান্টস](constants.md)
- [ফর লুপ](for.md)
- [ইফ/এলস](if-else.md)
- [সুইচ](switch.md)
- [অ্যারে](array.md)
- [স্লাইসেস](slices.md)
- [ম্যাপস](maps.md)
- [রেঞ্জ](range.md)
- [ফাংশন](function.md)

### অবজেক্ট, টাইপ ও মডিউল
- [পয়েন্টার](pointers.md)
- [স্ট্রাক্ট ও কাস্টম টাইপ](structs.md)
- [মেথড](methods.md)
- [ইন্টারফেস](interfaces.md)
- [এরর হ্যান্ডলিং](errors.md)
- [প্যাকেজ ও মডিউল](modules.md)

### আধুনিক গো ও স্ট্যান্ডার্ড ডেটা টুলস
- [জেনেরিকস](generics.md)
- [আধুনিক ইটারেটর](iterators.md)
- [জেসন ও ডাটা সিরিয়ালাইজেশন](json.md)
- [ফাইল ও আই/ও](io-files.md)
- [বেসিক এইচটিটিপি ক্লায়েন্ট ও সার্ভার](http.md)
- [স্ট্রাকচার্ড লগিং](slog.md)

### কনকারেন্সি, রিলায়েবিলিটি ও টেস্টিং
- [গোরুটিন](goroutines.md)
- [চ্যানেল](channels.md)
- [সিলেক্ট](select.md)
- [মিউটেক্স ও সিঙ্ক](mutex.md)
- [কনটেক্সট](context.md)
- [টেস্টিং ও বেঞ্চমার্কিং](testing.md)

---

## অনলাইনে পড়ুন (GitBook)

বইটি সরাসরি ব্রাউজারে পড়তে GitBook স্পেসে ভিজিট করুন:
- **[GitBook সংস্করণ](https://schlafer.gitbook.io/learn-golang-in-bangla)** *(অথবা আপনার কাস্টম ডোমেইন)*

---

## অবদান রাখুন (Contributing)

বইটি সম্পূর্ণ ওপেন-সোর্স। আপনার কোনো পরামর্শ, ভুল সংশোধন বা নতুন উদাহরণ যোগ করার ইচ্ছা থাকলে সানন্দে স্বাগতম:

1. এই রিপোজিটরিটি ফর্ক (Fork) করুন: [`schlafer/learn-golang-in-bangla`](https://github.com/schlafer/learn-golang-in-bangla)
2. নতুন ব্রাঞ্চ তৈরি করুন (`git checkout -b fix/chapter-typo`)
3. পরিবর্তন কমিট করুন (`git commit -m "docs: fix typo in slices"`)
4. আপনার ব্রাঞ্চে পুশ করে পুল রিকোয়েস্ট (Pull Request) পাঠান

---

## কৃতজ্ঞতা ও ক্রেডিট (Acknowledgements)

- **মূল উদ্যোগ ও কন্ট্রিবিউটর:** এই বইটির প্রাথমিক ভিত্তি ছিল [howtocode-dev/golang.howtocode.dev](https://github.com/howtocode-dev/golang.howtocode.dev)। মূল প্রজেক্টে যারা অবদান রেখেছিলেন তাদের প্রতি কৃতজ্ঞতা। মূল কন্ট্রিবিউটরদের তালিকা দেখতে পারেন [এখানে](https://github.com/howtocode-dev/golang.howtocode.dev/graphs/contributors)।
- **বর্তমান সংস্করণ ও রক্ষণাবেক্ষণ:** [schlafer/learn-golang-in-bangla](https://github.com/schlafer/learn-golang-in-bangla)।

---

## লাইসেন্স (License)

This work is licensed under a [Creative Commons Attribution-NonCommercial-NoDerivatives 4.0 International License](http://creativecommons.org/licenses/by-nc-nd/4.0/).

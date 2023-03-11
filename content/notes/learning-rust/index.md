---
title: "Rust: Day One Reflections"
date: 2023-03-10T03:40:09+03:00
draft: false
weight: 1
summary: It's not just about `println!("Hello, world!").. 🤌`
author: Evans Owamoyo
cover:
  image: rust-logo.png
  alt: Rust Logo
  relative: true
  side: right
categories:
  - Notes
  - Rust
tags:
  - LOTY2023
  - LanguageChallenge
comments: true
slug: rust-day-one
---
## Intro 🚪
Learning a new language is always a fun filled and mind opening experience, and I can't describe how excited I am right now🤓. This year 2023, I've chosen Rust 🦀 as the [language to learn](/tags/loty2023/) as part of the yearly [Language Challenge](/tags/languagechallenge/) goals.  
Although you may already have some knowledge of the Rust programming language, in this section, I will still introduce Rust and provide some information about it.   
Rust is a [systems programming language](https://en.wikipedia.org/wiki/System_programming_language) that was developed by Mozilla.   
The syntax of the language looks like that of C++, because of the namespaces, semicolons to terminate statements, and pointers.  
It also looks a bit like Swift especially with the way variable types, function return types and generics are defined. Both of the languages also have implicit function return, the keyword `self`, safe typing and optional types, and so on.
## Why Rust? 🦀
### Popularity and Love
Rust has been gaining momentum in the dev space for the past few years. According to the Jetbrain's 2022 Developer Survey and the Stackoverflow survey, Rust is the most promising programming language and for the past 2 years now, it has been one of the most loved programming languages by a very large margin.
### Linux
Linus Torvalds, the creator of Linux, has been a huge fan of Rust. He has also been a huge advocate for Rust and has been pushing for it to be used in Linux kernel development. As a backend developer who uses Linux, I think it's smart to learn Rust at this point in time.
### Speed
I love reading Medium posts daily and it's so rare not to see an article benchmarking Rust against other languages. Rust is known to be a fast language, and it's time I witnessed this first hand.
### Compiler
Rust has a very powerful compiler that helps you catch errors before they even happen, I have read all of these from daily feeds, a couple of times last year and I couldn't wait to see this in action.
### Memory Management
The main reason for learning Rust is to actually learn about memory management in Rust. It's method of memory management has been used in several notes and articles that I've read in people's work and if anything, I want to be able to understand what they are talking about.

---
Aside all these points, at first glance, I didn't like the Rust syntax. Frankly, I think I will still continue using Golang for most of my projects, but there is always something to be gained from learning a new language. 
## Relationship with Go ❤️ (LOTY2022)
Go and Rust actually have a lot in common, at least from what I've experienced.  
- Their compilers will scream at you when you create a variable you don't use.
- They are both compiled languages, known for speed and strong focus on performance.
- They are both modern languages with modern syntax that is easy to read and write.
- The usecases for Rust and Go also intersects as they are both used for low-level systems programming.
### Differences
- Rust beats Go in terms of Generics in my opinion because `<T>` definitely looks better than `[T]`. 
- Rust compiler is stricter than Go's compiler
- Rust is more complex, and might slow developers down for very large projects because of the strictness and memory issues that might arise.
- Go has more usecases than Rust, and is more popular than Rust. 
## Plan, Execution and Review
Without spending too much time talking about the languages themselves, let's talk about my learning plan and progress so far.
### Study Plan 📚
The Rust language has a very detailed documentation on their official website. That is my starting point. I will be reading the documentation and following along with the examples. I will also be reading the Rust book.
Since I do not plan to use Rust for any of my projects, I might solve some algos with Rust on Exercism at least.
### Day 1 in summary 📅
Today is the first day of the Language Challenge and I have already spent a lot of time reading the Rust documentation. 
I installed the Rust toolchain with `brew` so that I can update it easily, should a new version be released. I also installed the VSCode extension for Rust.
I have also read the Rust book and I am currently on the fourth chapter where the reference and borrowing concepts are introduced.  
I think it's a memory management model that makes sense, and I am looking forward to learning more about it.
### Review 📊
My take on Rust so far is that it requires focus and attention to detail compared to other programming language and I think this is why folks say it's a bit complex. I won't be blogging my progress daily because I don't want to bore you with the details, but I will be posting a summary of my progress at the end of the month. 

---
Thanks for reading, and I hope you enjoyed this post. If you have any questions, feel free to reach out to me on [Twitter](https://twitter.com/lordvidex) or [LinkedIn](https://www.linkedin.com/in/evansowamoyo/). Also, don't forget to like the post if you enjoyed it. 

Peace ✌️



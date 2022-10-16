---
title: "Writing faster Java algorithms"
summary: Unlocking the C speed in your Java program
description: |
    Optimization techniques for writing Java programs
date: 2021-01-15T04:34:33+03:00
draft: false
weight: 2
cover:
    image: "images/speed.png" # image path/url
    alt: "Speed Image" # alt text
    relative: true # when using page bundles set this to true
author: 'Evans Owamoyo'
categories: [Java]
tags: [algorithms]
---

**C Speed?**

{{< figure src="images/steve-really.gif" alt="Really?" align=center >}}

I started competitive programming in C++ because it is the most common language on competitive programming sites out there. Of course, due to the famous STL library and the **speed** you get out of a C++ program. Switching to Java for writing algorithms has been a totally nice experience, and I found some ways to increase the speed of your Java algorithm in contests and projects generally.

## **Why write in Java?**

* You use Java everyday for day-to-day tasks like Android or server-side development or probably Java was your first love. :))

{{< figure align=center src=images/weird-grin.gif caption="Me simping for a programming language, D:)" >}}

* Most Data Structures you’ll need are easier to implement and readily available in the `java.util.*` package or `java.io.*` as opposed to C++ for example, where you have to import all the libraries you will need from STL or alternatively `#include <bits/stdc++.h>` header file which contains all the major classes you will need. The downside is that this header file is not present in C compilers like Microsoft Visual Studio C++ or Xcode’s Clang compiler.

* Most of the Java algorithms you will write will be bug free because the syntax is human friendly and the IDE will most times save you from yourself. There is also the wonderful debugging experience you get from popular IDEs like [IntelliJ](https://www.jetbrains.com/idea/).

* Extensive documentation, age, libraries, resources and the ability to peek at the source code.

* ## WHY NOT?

*Let’s get down to the speed tips:*

{{< figure src="images/sport-car.gif" alt="sport-car" >}}

### **1. Improve your IO operations**

It is far easier to receive input from the terminal with Java's `Scanner` class but it is relatively slower than using `BufferedReader` because :

* **BufferedReader** has larger buffer than **Scanner**: 8KB to 1KB

* **Scanner** uses regular expressions internally to read and parse Strings to their types in functions `nextDouble() , nextInt() `,etc.. which we use when reading numbers from file for example, and well, **regExp** is slow.
> Below is a demo showing the time taken by Scanner and BufferedReader respectively to read in integers from a file containing 2,000,000 numbers.

**Scanner Implementation**
{{< figure src="images/test-class.png" alt="Definition of the Test class" caption="**speed**::: 1.53 s user, 0.15s system, 1.289 total" align=center >}}

**Equivalent BufferedReader implementation**

Let’s first define a static class named `FastScanner` , which contains a `BufferedReader` and a `StringTokenizer`

{{< gist lordvidex 0fe79ccaed2f06a60ad6927d3afa524e >}}

> now you can add methods to return common types like `int` , `double` , `char[]` etc and don’t forget to always **close** the `BufferedReader`.

{{< figure src="images/test-methods.png" alt="Helper methods created in the Test class" align=center caption="Full content of FastScanner class is present at the bottom of page" >}}

> You can add methods to the FastScanner class to your requirement, in most contests, you’ll deal with integers, long, double, char[] ,strings.

\# **speed test**
{{< figure src="images/array-test.png" alt="Result of the test" align=center caption="VOILA!!!! this is x4 faster now" >}}

> What about outputs?

I use the **PrintWriter** class in place of the *traditional* **System.out.println(), **it is roughly twice faster, nothing much… but speed is speed :)

** An **important** performance tip will be to **group pieces of Strings together** to form a single String (*using StringBuilder()*) before printing out from the output stream.

![](https://cdn-images-1.medium.com/max/3436/1*tFN8hndjpRwOX-QryIJSSA.png)

### **2. Use primitive types where necessary in place of Objects**

Primitive values are stored in the **stack**, in the computer memory WHILE Objects are stored in the **heap. **The most important thing to know about them in terms of speed is that**, **the** stack **is a faster memory area than the **heap, **hence we have to consider this in our algorithms and use primitive types instead of object types when and where possible. The test here is actually going to be a short one:

![This is like x40+ time difference in nanoTime just summing up numbers from 0 to 999. More complex tasks will cause greater variations in time](https://cdn-images-1.medium.com/max/3406/1*oDNQ_Q0JAC7LN_F3HL9JTg.png)*This is like x40+ time difference in nanoTime just summing up numbers from 0 to 999. More complex tasks will cause greater variations in time*

### **3. Use Arrays in place of ArrayLists where necessary**

**Arrays** should be used in place of **ArrayLists** where necessary especially when the **size **of the array is known. Aside the speed of using Arrays over ArrayLists, it is actually more convenient to get a value in the array using `arr[i][j]` instead of `arr.get(i).get(j)` for Lists.

![](https://cdn-images-1.medium.com/max/3130/1*8SJi1r__vmotUmSQUDw0qA.png)
> Even though the ArrayList was also initialised with the **size, **Array will of course be faster because of point 2 above. NOTE that you should use **ArrayList** when you don’t know the number of items that will be stored, because Lists are mutable.

### **4. Know when to use String “+” operator, StringBuilder and char[]**

Manipulating Strings is a very important part of algorithms and data structure. This is the simple rule I follow when working with Strings:

* When printing or storing already known String values, concatenate them using “+”. For example, in
```Java
String myString = "I "+"am "+"a "+"Java Champion!";
// and in
System.out.println("I "+"am "+"a "+"Java Champion!");
```

The String *“I am a Java Champion!”* is actually stored as a single String value in the **String pool**, and it doesn’t make sense here creating a **StringBuilder** for something like this.

* Use **StringBuilder** when you are repeatedly adding, removing, or replacing Characters/Strings.

* AVOID using “+=” on Strings within loops.

* Use char[] when you’ll be mutating characters in the string a lot but the size of this string never changes. For example, getting all possible word permutations of Strings that can be formed from the word **“javastrings”. **There are** 9979200** possible permutations and using a **char[]** would make it really easy to swap characters without having to rebuild a new **string**.
```Java
String str = "foobar";
//! `.toCharArray()` converts strings to character arrays
char[] characters = str.toCharArray();
//! characterArrays can be converted back to Strings
// using `String.valueOf()`;
String finalString = String.valueOf(characters);
```

### **Finally…**
{{< figure src="images/finally.gif" align=center  >}}

> These optimisations will make your algos run faster. However, your algorithm has to be effective in the first place. Irrespective of programming language, **time complexity** and **space complexity** of an algorithm must be OPTIMAL.  

> My template for competitive programming in Java:
{{< gist lordvidex cad23270f0c1b8684cb5a3f4b8839a56 >}}

Happy coding !! ✌️🤖👨🏽‍💻

### **References:**

* [Difference between scanner and BufferedReader class in java](https://www.geeksforgeeks.org/difference-between-scanner-and-bufferreader-class-in-java/)

* [https://www.geeksforgeeks.org/stringtokenizer-class-java-example-set-1-constructors/](https://www.geeksforgeeks.org/stringtokenizer-class-java-example-set-1-constructors/)

* [https://www.guru99.com/stack-vs-heap.html](https://www.guru99.com/stack-vs-heap.html)

* Original FastScanner template gotten from @[agminadur](https://codeforces.com/profile/Agnimandur)



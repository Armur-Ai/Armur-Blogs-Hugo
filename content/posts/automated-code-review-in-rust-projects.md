---
title: 'LLMs Enhancing Security: Automated Code Review in Rust Projects'
date: 2024-07-30T15:20:22+05:30
image: 'https://wallpaperaccess.com/full/416144.jpg'
tags: ["security", "rust"]
---
Recently, we’ve been seeing , how simple bugs can result to huge errors in software. It shows that there should be no room for errors in the code we write. Rust, a systems programming language known for its focus on memory safety and concurrency, has gained significant traction among developers seeking to build robust and secure applications. However, even with Rust's inherent safety features, the complexity of modern software systems demands additional layers of security scrutiny. This is where Large Language Models (LLMs) are making a substantial impact, particularly helping developers by automated code reviews.

## Introduction

Rust's design philosophy prioritizes memory safety and thread safety, making it an excellent choice for developing secure systems. However, as projects grow in size and complexity, maintaining high security standards becomes increasingly challenging. Traditional code review processes, while valuable, can be time-consuming and prone to human error. Enter LLMs – sophisticated AI models that can understand and analyze code at a level approaching human comprehension.
The Role of LLMs in Code Review

LLMs bring a new dimension to code review by leveraging their vast knowledge of programming patterns, best practices, and common vulnerabilities. Unlike rule-based static analysis tools, LLMs can understand context, identify subtle issues, and even suggest improvements that align with idiomatic Rust.

### Advantages of LLM-powered code review include:
Consistency: LLMs apply the same level of scrutiny to every line of code, avoiding human fatigue.
Speed: They can analyze large codebases in a fraction of the time it would take a human reviewer.
Continuous learning: As new vulnerabilities and best practices emerge, LLMs can be updated to incorporate this knowledge.
When it comes to Rust projects, LLMs can focus on several key areas:
a) Memory safety checks: While Rust's ownership system prevents many memory-related issues, LLMs can identify potential misuses of unsafe code or complex ownership patterns that might lead to vulnerabilities.
b) Concurrency and thread safety analysis: LLMs can detect race conditions, deadlocks, and other concurrency issues that might slip past Rust's compile-time checks.
c) Identification of unsafe code blocks: LLMs can scrutinize unsafe blocks, ensuring they're necessary and properly implemented.
d) Detection of potential integer overflow vulnerabilities: Although Rust checks for integer overflow in debug builds, LLMs can identify scenarios where overflow might occur in release builds.
Using LLM agents on Rust Code Review
We will be taking a look at the following example. 
The provided Rust program demonstrates a simple multi-threaded counter. The program creates ten threads, each of which increments a shared counter 1,000 times. The counter is wrapped in an AtomicUsize and shared between threads using Arc to ensure that each thread can access and modify the counter safely.
The program should increment the counter a total of 10,000 times (10 threads * 1,000 increments each). Once all threads have completed their execution, the final value of the counter should be printed.
Result: 10000
Here’s the example

```rust

// src/main.rs

use std::thread;
use std::sync::Arc;
use std::sync::atomic::{AtomicUsize, Ordering};

fn main() {
    let counter = Arc::new(AtomicUsize::new(0));
    let mut handles = vec![];

    for _ in 0..10 {
        let counter = Arc::clone(&counter);
        let handle = thread::spawn(move || {
            for _ in 0..1000 {
                let current_value = counter.load(Ordering::Relaxed);
                counter.store(current_value + 1, Ordering::Relaxed);
            }
        });
        handles.push(handle);
    }

    for handle in handles {
        handle.join().unwrap();
    }

    println!("Result: {}", counter.load(Ordering::Relaxed));
}
```

As mentioned, the output should be 10000.  Let’s run the program 3 times. Note, you might be getting different numbers from the ones written here.
1st run: 7790, 2nd run: 6530, 3rd run: 6350.
Based on the results, we are seeing different changes in the output. Let’s do a code review and see the issues we’re getting.

### Results and Fixes
We got that the  Rust code creates multiple threads that increment a shared counter. While it runs without crashing and produces the expected result, it has security and correctness issues related to the use of atomic operations. Below, we outline these issues and provide solutions.
Relaxed Memory Ordering: The code uses Ordering::Relaxed for both loading and storing the counter's value. Relaxed ordering does not impose any synchronization or ordering constraints on other operations. While AtomicUsize ensures atomicity, Ordering::Relaxed allows for potential visibility issues across threads, leading to race conditions and undefined behavior. Using Ordering::Relaxed basically means that the increment operations might not be properly synchronized across threads. This could cause the final value to be incorrect because threads may not see the most up-to-date value of the counter.
Solution: To correct this, you should use Ordering::SeqCst (Sequential Consistency) for both load and store operations. This ordering provides the strongest form of synchronization, guaranteeing that all operations happen in a strict order across threads, thus preventing visibility issues.
Fetch and Increment Optimization: Even with Ordering::SeqCst, the process of loading a value, incrementing it, and then storing it back isn't optimal because it's not atomic. If multiple threads execute counter.load and counter.store simultaneously, they might load the same value, leading to lost updates.
Solution: To atomically increment the counter, use the fetch_add method provided by AtomicUsize. This method ensures that the increment operation is performed atomically.
Atomic Operations: Replacing non-atomic load and store with the atomic fetch_add operation ensures that increment operations are performed safely across multiple threads, without the need for additional synchronization mechanisms like locks.

Here’s the provided code with fixes:
```rust
// src/main.rs

use std::thread;
use std::sync::Arc;
use std::sync::atomic::{AtomicUsize, Ordering};

fn main() {
    let counter = Arc::new(AtomicUsize::new(0));
    let mut handles = vec![];

    for _ in 0..10 {
        let counter = Arc::clone(&counter);
        let handle = thread::spawn(move || {
            for _ in 0..1000 {
                counter.fetch_add(1, Ordering::SeqCst);
            }
        });
        handles.push(handle);
    }

    for handle in handles {
        handle.join().unwrap();
    }

    println!("Result: {}", counter.load(Ordering::SeqCst));
}
```

Now, when you run this, you get the desired output which is 10000. You can try it again, still the same answer. Manual review of understanding might have costed you a lot of time but using LLMs identifies these issues, and solves them. 

Implications on a large scale
Now this is a simple example but if these issues are found on a large scale software, some of the following could happen:
Potential Denial of Service (DoS):
If counters are used for rate limiting or resource allocation, inaccurate counts could lead to unintended resource exhaustion.
Attackers might exploit the race condition to bypass rate limits, potentially overwhelming the system.
Scalability Issues:
As concurrency increases, the likelihood and impact of race conditions grow exponentially.
The system may become increasingly unstable or unreliable as it scales up, with more threads competing for the shared counter.
Performance may degrade unexpectedly under high load due to increased contention and lost updates.
Potential Financial Losses:
If the counter represents financial transactions or billable events, lost updates could directly translate to revenue loss.
Inaccurate data could lead to incorrect billing, potentially resulting in either undercharging (financial loss) or overcharging (legal risk).
Unpredictable Behavior in Critical Operations:
Critical operations relying on accurate counter values may behave erratically.
Decision-making processes based on these counters could lead to incorrect actions, potentially affecting system integrity or user experience.
Inconsistent behavior could undermine trust in the system, especially in mission-critical applications.
These implications highlight how seemingly small concurrency issues can have far-reaching consequences in large-scale systems, potentially affecting security, reliability, and financial stability.

### Conclusion
Large Language Models are improving code review processes, particularly in security-critical languages like Rust. By providing fast, consistent, and context-aware analysis, LLMs are becoming an invaluable tool in the developer's arsenal. While they cannot replace human expertise entirely, LLMs significantly enhance our ability to create secure, robust Rust applications. As these technologies continue to evolve, their integration into development workflows will likely become standard practice, further strengthening the security posture of Rust projects worldwide.
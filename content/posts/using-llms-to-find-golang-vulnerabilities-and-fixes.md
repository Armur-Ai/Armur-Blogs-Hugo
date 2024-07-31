---
title: "Using LLMs to Find Golang Vulnerabilities and Fixes "
date: 2024-07-30T15:20:22+05:30
image: "https://wallpaperaccess.com/full/416197.jpg"
tags: ["security", "go"]
---

Ensuring the security of software applications is becoming more important as technology evolves. Go, known for its simplicity and efficiency, is a popular choice for building high-performance applications. However, like any programming language, Go is prone to security vulnerabilities. Detecting and fixing these vulnerabilities early is crucial to safeguarding applications.

We will be using Armur which uses LLM Agents for Code Vulnerability Scanning. Armur is a platform designed to help developers find and fix security vulnerabilities in your code. By using Large Language Models (LLMs), Armur can analyze code and identify potential vulnerabilities more effectively. This article will guide you on how to use Armur with LLMs to improve the security of your Go applications.

### Finding vulnerabilites on your Go code

We will be focusing on a golang code snippet as follows.

```golang
package main

import (
    "fmt"
    "sync"
)

var (
    data = make(map[string]string)
    mu   sync.Mutex
)

func saveUserData(key, value string) {
    defer mu.Unlock()

    data[key] = value
    fmt.Printf("Saved: %s = %s\n", key, value)
}

func main() {
    // Example usage
    saveUserData("admin", "super_secret_password")
    saveUserData("config", "<script>alert('XSS')</script>")
}
```

The code defines a basic program that saves user data into a map while ensuring thread safety using a mutex. The `data` variable is a map that stores key-value pairs of strings, and mu is a mutex used to control access to the map. The `saveUserData` function adds a key-value pair to the data map and prints the saved data to the console. It uses `mu.Unlock()` to release the mutex lock at the end of the function, ensuring that concurrent access to the map does not cause data races. The main function demonstrates the usage of saveUserData by saving an admin password and a configuration script.

Someone without proper experience my use this code in their projects, or in production even if it’s full of vulnerabilities and has room for improvements. Let’s assume you don’t see any problems and decide to use armur agents either way for double checking.

### Findings using LLM Agents

Guess what, the code is not secure and our agents picked that out. Let’s go over all the vulnerabilities as well as solutions.

1. Mutex Locking Issue
   **Problem**: The saveUserData function uses a sync.Mutex for thread safety. However, the defer mu.Unlock() statement is employed without a prior mu.Lock() statement. This results in incorrect usage which could lead to a panic at runtime.
   **Solution**: Ensure correct usage of the mutex by locking it before accessing the shared resource.

2. Plaintext Password Storage
   **Problem**: Storing passwords as plaintext (e.g., admin:super_secret_password) is highly insecure and can lead to credential theft if the data is accessed by an unauthorized party.
   **Solution**: Use a hashing function to store passwords securely. Libraries like bcrypt can be used for this purpose.

3. Cross-Site Scripting (XSS)
   **Problem**: The application allows potentially dangerous inputs (e.g., <script>alert('XSS')</script>) to be stored, which could lead to XSS attacks if the data is used in HTML contexts.
   **Solution**: Escape or sanitize inputs that are meant to be displayed in web contexts to prevent execution of harmful scripts. In the context of a web application, you would typically use a library like html/template in Go.

Now we have went over all the issues that were present on the code.

### Full Code with Security Fixes

Below is the complete code incorporating all the suggested fixes:

```golang
package main

import (
    "fmt"
    "golang.org/x/crypto/bcrypt"
    "html"
    "sync"
)

var (
    data = make(map[string]string)
    mu   sync.Mutex
)

func hashPassword(password string) (string, error) {
    hashedPassword, err := bcrypt.GenerateFromPassword([]byte(password), bcrypt.DefaultCost)
    if err != nil {
        return "", err
    }
    return string(hashedPassword), nil
}

func saveUserData(key, value string) {
    mu.Lock()
    defer mu.Unlock()

    if key == "admin" {
        hashedPassword, err := hashPassword(value)
        if err != nil {
            fmt.Println("Error hashing password:", err)
            return
        }
        data[key] = hashedPassword
    } else {
        data[key] = html.EscapeString(value) // Escape potentially dangerous characters for HTML
    }
    fmt.Printf("Saved: %s = %s\n", key, value)
}

func main() {
    // Example usage
    saveUserData("admin", "super_secret_password")
    saveUserData("config", "<script>alert('XSS')</script>")
}
```

### Summary

By implementing the above security best practices, we can significantly improve the security posture of the code:
Mutex Locking: Ensuring proper mutex lock/unlock sequences to avoid race conditions and panics.
Password Hashing: Storing sensitive information like passwords securely using industry-standard hashing algorithms (e.g., bcrypt).
XSS Protection: Preventing XSS vulnerabilities by escaping potentially unsafe characters before storing input data that might be rendered in an HTML context.

Following these practices will make the code more secure and robust against common security threats.
Additionally, Large Language Models (LLMs) play a crucial role in identifying these vulnerabilities and providing solutions. LLMs can analyze code patterns, detect security flaws, and suggest best practices to mitigate potential risks, enhancing the overall security of the application.

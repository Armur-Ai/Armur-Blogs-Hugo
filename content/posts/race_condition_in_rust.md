---
title: Race condition in std::fs::remove_dir_all in Rust
date: 2024-06-03T15:20:22+05:30
image: 'https://i.ibb.co/WvHLRbL/wp8769017-old-paintings-wallpapers.jpg'
---

## Introduction

In Rust, the `std::fs::remove_dir_all` function is vulnerable to a race condition that can lead to unintended following. This type of vulnerability is clasified under CWE-363, which describes scenarios where a race condition allows an attacker to manipulate file links, potentially leading to unauthorized file or directory access.

This issue has been assigned `CVE-2022-21658`.

## An Overview of this vulnerability

The vulnerability in `std::fs::remove_dir_all` presents a significant security risk. To illustrate, consider an attacker who has obtained unprivileged access to a system and wants to delete a system directory called sensitive/, but lacks the necessary permissions and privileges.

If `std::fs::remove_dir_all` followed symbolic links, the attacker could find a privileged program that removes a directory they have access to, such as temp/. By creating a symlink from temp/foo to sensitive/ and waiting for the privileged program to delete foo/, the program would follow the symlink from temp/foo to sensitive/ during recursive deletion, resulting in sensitive/ being deleted.

## How Symbolic Links Work

A symbolic link (or symlink) is a file that points to another file or directory. This is similar to shortcuts in Windows systems. Symlinks allow files or directories to be accessed indirectly through another path.
Here's a simple Rust code example using `std::fs::remove_dir_all`:

```rust
use std::fs;

fn main() -> std::io::Result<()> {
    fs::remove_dir_all("/some/dir")?;
    Ok(())
}
```

However, this protection was implemented incorrectly in the standard library, resulting in a flow which is a race condition.

Instead of instructing the system not to follow symlinks, the standard library first checked whether the item to be deleted was a symlink. If it wasn't, the function would proceed to recursively delete the directory.
This created a race condition: an attacker could replace a directory with a symlink between the check and the deletion.

Although this attack might not succeed on the first attempt, an experiment showed it could be reliably executed within a few seconds.

## How this vulnerability may be addressed

To address this vulnerability, developers should update to the latest version of Rust where this issue has been patched. Ensuring that your code does not rely on the flawed behavior of `std::fs::remove_dir_all` is crucial for maintaining security.

Here is an example of safe directory deletion that avoids the race condition:

```rust
use std::fs;
use std::path::Path;
fn safe_remove_dir_all(path: &Path) -> std::io::Result<()> {
    if path.is_symlink() {
        fs::remove_file(path)
    } else {
        for entry in fs::read_dir(path)? {
            let entry = entry?;
            let path = entry.path();
            if path.is_dir() {
                safe_remove_dir_all(&path)?;
            } else {
                fs::remove_file(path)?;
            }
        }
        fs::remove_dir(path)
    }
}
```

All versions of Rust before the patch addressing CVE-2022-21658 are affected if your code or its dependencies use std::fs::remove_dir_all with potentially untrusted input. It is crucial to update to the latest version to mitigate this vulnerability.
This post highlights the importance of correctly handling symlinks in filesystem operations. Developers are urged to update to the latest version of Rust and review their use of std::fs::remove_dir_all to ensure it handles symlinks securely.

## What Armur is doing about this?
These are the kinds of vulnerabilities that [Armur](https://armur.ai) will protect you against. Armur has a set of developer security tools powered by LLMs and will help you scan your code scan thoroughly, identifying any security issues that may be present. Furthermore, it offers detailed recommendations on how to fix these issues, ensuring that your code remains robust and secure. Available tooling support for languages includes Rust, Go, Python, Javascript and Solidity.

To stay updated with the latest security discussions, you can join our [discord server](https://discord.com/invite/qGMMmgFnZD) where we share valuable insights and learning resources such as SecOps, DevSec and Red Teaming. Additionally, you can try out [Armur](https://armur.ai) and get free 100 credits.

![Armur Image](https://i.imgur.com/q14I8yd.png)
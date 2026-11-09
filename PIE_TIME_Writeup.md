# PIE TIME — CyLab Security Academy

**Course:** CPSC 4584 — Special Topics in Information Security  
**Date:** November 8, 2026  
**Category:** Binary Exploitation  
**Difficulty:** Easy

## 1. Purpose

This writeup shows how I completed the PIE TIME challenge. I had to figure out what address to enter so the program would run the `win()` function. I looked at the code, connected to the program, and used the address it gave me to work out the right one. This challenge was about binary exploitation and memory addresses.

## 2. Scope and Environment

I used the terminal on the CyLab Security Academy website. I looked at the source code they gave us and used the `nc` command to connect. I didn't use Kali Linux. The challenge had a time limit, so I worked inside that session.

## 3. Investigation Procedure

### Step 1: Looking at the Code

First, I looked at the source code to see what I was supposed to do. I noticed the `win()` function was what I needed to reach, and the program printed the address of `main()` before asking for an address to jump to.

### Step 2: Connecting to the Program

Next, I used the CyLab webshell to connect:

```bash
nc chatelaine.cylabacademy.net 25894
```

The program showed an address for `main()` and asked me to enter a different address. The address can change between runs because the binary uses PIE (Position Independent Executable).

### Step 3: Finding the Right Address

The difference between the `main()` and `win()` function offsets was `0xa2`. I subtracted that amount from the address of `main()` that the program printed:

```text
win address = leaked main address - 0xa2
```

I entered the calculated address when the program asked where to jump. It printed `You won!`, which showed that I reached the `win()` function.

*I kept the actual flag and full terminal screenshots in my private Blackboard evidence report, not this public writeup.*

## 4. Tools Used

| Tool or Command | What I used it for |
|---|---|
| CyLab webshell | Running the challenge in the browser |
| `nc` | Connecting to the challenge program |
| Provided source code | Finding the `win()` function and understanding the program |

## 5. Troubleshooting and Decision-Making

One thing I was confused about was what address I was supposed to enter. At first, I didn't understand the example address. Once I understood that I needed to use the address of `main()` and subtract `0xa2`, I was able to enter the right one.

## 6. What I Learned

I learned a little more about how memory addresses work and how a program can be made to run a different function. This connects to our Maplewood work because we were also looking at security problems in programs. Next time, I would look through the code more carefully before starting.

---

**Note:** This public version does not include the captured flag, personal information, or private evidence screenshots.

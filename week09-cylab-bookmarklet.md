# CyLab Security Academy — Bookmarklet Challenge

**Course:** CPSC 4584 — Special Topics in Information Security  
**Date:** November 1, 2026  
**Challenge:** Bookmarklet  
**Category:** Web Exploitation (Web)  
**Difficulty:** Easy  
**Platform:** CyLab Security Academy  

> **GitHub version:** This is a public-friendly write-up. I left out the challenge flag, my student ID, and private submission screenshots. My full evidence report is for Blackboard only.

## 1. Purpose

The evidence report documents the Web Exploitation challenge I worked on in CyLab Security Academy. The purpose of this challenge was to learn how JavaScript works in a web browser and how it can be used to find hidden information. The methodology I used was to first open the provided website, examine the bookmarklet code, and follow the steps to run the JavaScript in the browser. I also used the challenge hints to help me understand what I needed to do. The main goal was to find the hidden flag while getting a better understanding of web exploitation and how JavaScript can be used on websites.

## 2. Scope and Environment

For this challenge, I used CyLab to work on the web exploitation assignment. The main tools I used were the CyLab webshell, my web browser, and the browser console. I used the webshell to access the website after starting the instance and then used the browser console to work with the JavaScript code. I only used the website and tools provided for the challenge. One limitation was my own knowledge because I forgot about the console part of a website, so it was a self-limitation.

## 3. Investigation Procedure

### Step 1: Opening the Challenge Website

First, I opened the Bookmarklet challenge on CyLab Security Academy and read the instructions and hints to see what I needed to do. Then I used the CyLab webshell and entered a `wget` command with the challenge's temporary website address. The response showed **200 OK**, and the HTML file downloaded successfully. After that, I opened the website using the provided link.

```bash
# Example format; the instance link changes between sessions.
wget http://<challenge-instance-address>/
```

### Step 2: Looking at the JavaScript Code

Next, when I opened the website, I noticed some JavaScript code inside a small text box. I looked at the code and copied it because I needed to figure out how to use it to find the hidden flag. After that, I opened my browser's Developer Tools and went to the **Console** tab so I could work with the JavaScript code.

### Step 3: Running the Code and Finding the Flag

Lastly, I used the browser console to run the JavaScript code from the website. I did this to see what the code would do and whether it would show the hidden flag. After running it, I checked the results, then went back to CyLab Security Academy and entered the flag in the answer box. The challenge showed that it was solved. I am leaving the actual flag out of this public write-up.

## 4. Tools Used

| Tool or Command | Purpose |
|---|---|
| CyLab webshell / `wget` | I used this to access the challenge website and download the HTML file. |
| Web browser | I used this to open the website and look at the JavaScript code. |
| Browser console / Developer Tools | I used this to run the JavaScript code and look for the hidden flag. |

## 5. Challenge Result

**Result:** Challenge solved. The actual flag and full screenshots are saved only in my private Blackboard evidence report.

## 6. Troubleshooting and Decision-Making

One limitation was my own knowledge because I forgot about the console part of a website, so it was more of a self-limitation. At first, I was confused about what I was supposed to do with the JavaScript code that was provided. I tried looking at the website and reading the hints, but I still wasn't completely sure how to get the flag. After looking into it more, I remembered that I could use the browser console through Developer Tools. Once I figured that out, I was able to run the JavaScript code and get a better understanding of how the challenge worked. This also helped me remember something I had learned before but just forgot about.

## 7. What I Learned

One thing I did learn was that there's always a way to find something out. We just have to go searching for it and try different things until we figure it out. This challenge also reminded me how useful the browser console can be when working with JavaScript and websites. This connects to the Maplewood lecture because we are learning how to investigate security issues and use different tools to find information to help with the case. Next time, I would try to remember the tools I already know about and not overthink the challenge too much.

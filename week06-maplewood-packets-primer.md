# Week 6: Maplewood Security Team Playbook — Packets Primer

**Course:** CPSC 4584  
**Platform:** CyLab Security Academy  
**Challenge:** Packets Primer  
**Category:** Forensics  
**Difficulty:** Medium  
**Submission:** Public-safe GitHub playbook

## 1. Challenge Overview

For Week 6, I worked on the Packets Primer challenge, which introduced me to analyzing a packet capture file. The goal was to download the provided `.pcap` file, examine its contents, and identify a hidden flag. This challenge helped me understand how network captures can contain readable information and why checking network traffic can be useful during a security investigation.

## 2. Authorized Scope

I completed the investigation in the CyLab Security Academy webshell using the packet capture supplied by the challenge. I only examined the assigned file and did not scan or access any outside systems. This GitHub playbook leaves out the captured flag, private submission screenshots, and student information.

## 3. Investigation Steps

### Step 1: Download the packet capture

I used `wget` with the download link provided by the challenge to save the packet capture in the webshell. After that, I checked that the file was there.

```bash
ls -lh network-dump.flag.pcap
```

The file appeared in the directory, so I knew the download worked and could move on to examining it.

### Step 2: Look for readable text

I used `strings` to pull readable text out of the packet capture without needing to open it in Wireshark.

```bash
strings network-dump.flag.pcap
```

The output included some short random-looking text and a line that looked like it might contain the flag. I kept the flag value out of this public report.

### Step 3: Try searching for a specific pattern

At first, I assumed the flag would use the common `picoCTF` prefix. I tried filtering the output with `grep`.

```bash
strings network-dump.flag.pcap | grep 'picoCTF'
```

That command did not return a match. I also tried searching for a portion of the text I thought I saw, but it did not match exactly either. This showed me that I needed to inspect the actual output instead of guessing the text.

### Step 4: Inspect the file more closely

I used a hex dump to see the file's bytes alongside their readable characters.

```bash
xxd network-dump.flag.pcap
```

This helped me locate the interesting text in the capture. I then ran `strings` again with a minimum string length of three characters.

```bash
strings -a -n 3 network-dump.flag.pcap
```

The output showed the candidate flag text in a more readable way. I recorded that evidence for my private Blackboard report, rather than publishing the flag on GitHub.

## 4. Tools and Commands Used

- **CyLab webshell:** Ran Linux commands in the authorized challenge environment.
- **`wget`:** Downloaded the assigned packet capture from the provided challenge link.
- **`ls -lh`:** Confirmed that the file downloaded successfully.
- **`strings`:** Displayed readable text from the capture.
- **`grep`:** Filtered text to look for a matching flag pattern.
- **`xxd`:** Examined the raw bytes and readable text in hex format.

## 5. Troubleshooting and Decision-Making

One problem I ran into was that my `grep` command gave me no results. I was searching for a prefix I expected to see, but the challenge used a different format. I also noticed that some characters were hard to read in the terminal. Instead of continuing to guess, I checked the file with `xxd` and then used `strings -a -n 3` to view the text again. This helped me narrow down what was actually in the capture.

## 6. Security Connection

This challenge connects to network forensics because it shows that packet captures may reveal information exchanged across a network. It also reminded me that analysts need to look carefully at their evidence before deciding what it means. Tools like Wireshark are useful for more detailed packet analysis, but basic Linux commands can still help with an initial investigation.

## 7. What I Learned

I learned how to download and inspect a `.pcap` file using Linux commands. The biggest lesson for me was not to assume that every flag follows the same format. Next time, I would inspect the readable content first and use filters based on what I actually find. I would also use Wireshark if I needed to examine individual packets or network protocols in more detail.

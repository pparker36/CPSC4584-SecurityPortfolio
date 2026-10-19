# Week 7 Maplewood Security Team Playbook

**Topic:** Network Packet Analysis with Wireshark  
**Course:** CPSC 4584 – Special Topics in Information Security  
**Platform:** CyLab Security Academy  
**Challenge:** Wireshark doo dooo do doo...  
**Category:** Forensics  

## Overview

For Week 7, I practiced looking through network traffic using Wireshark. The goal was to find hidden information inside a provided packet capture. I used TShark to check the traffic first, then opened the capture in Wireshark to narrow down the packets and look for clues.

## Tools Used

- **CyLab Webshell:** Downloaded and checked the provided capture file.
- **TShark:** Looked at the packet traffic from the command line.
- **Wireshark:** Filtered the HTTP traffic and inspected a TCP stream.
- **ROT13:** Helped me understand an encoded message found in the traffic.

## Investigation Steps

### Step 1: Open the capture

I downloaded the packet capture provided by the challenge and opened it in Wireshark. The file had **987 packets**, so I needed a way to focus on the useful traffic instead of checking every packet one by one.

### Step 2: Filter the HTTP traffic

A lot of the HTTP traffic was related to Windows management requests, and some of its content was encrypted. To focus on the other HTTP packets, I used this Wireshark display filter:

```wireshark
http && !(http.request.uri contains "/wsman/")
```

The filter brought the results down to **four packets**, which made them easier to check.

### Step 3: Inspect the stream

I looked at the HTTP response in **packet 827**, then used **Follow → TCP Stream** to see the full conversation. The response included a message that looked scrambled. I recognized that it was encoded using ROT13 and decoded it as part of the challenge. I'm leaving out the actual message and flag because this is a public-safe playbook.

## Troubleshooting

At first, I searched the capture for the flag text in the Webshell, but nothing came up. I also ran into a lot of encrypted and repeated traffic. Filtering out the WSMan requests helped me focus on the small number of readable HTTP packets. Once I checked the TCP stream, the clue was easier to spot.

## What I Learned

This challenge helped me get more comfortable filtering packets and following TCP streams in Wireshark. It also connected to our Maplewood network analysis work because both involve looking for unusual details in traffic. I learned that ROT13 can make text look hidden, but it isn't real encryption. Next time, I'd start with protocol and display filters earlier to save time.

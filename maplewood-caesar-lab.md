Analyst: Pierre Parker

Date: 9/27/26

Challenge Name: Caesar

Category: Cryptography

Difficulty: Medium

Platform: CyLab Security Academy

Career Track: Security Operations

Artifact Type: Public Portfolio SOP


#1. Challenge Overview
This challenge was all about breaking down substitution ciphers and looking closely at how the classic Caesar cipher handles character shifts when decoding. The main security takeaway is straightforward: if your encryption relies entirely on shifting alphabet letters by a fixed number without adding real complexity or a massive keyspace, it really isn't secure. Since a Caesar cipher simply rotates letters forward or backward by a set amount, it falls apart immediately against basic brute-forcing, which there are websites that do in seconds. The big learning goal here was to get comfortable with manual and automated decryption techniques, understand how these patterns show up in captured files, and figure out how to pull out hidden plaintext payloads without knowing the shift key beforehand.


#2. Purpose and Methodology
This Standard Operating Procedure walks through the exact investigation steps, testing process, and decryption workflow I used to analyze a ciphertext file in a secure lab setup. Instead of just guessing random keys, I went with a step-by-step, repeatable game plan: download and check the file structure, run an automated brute-force tool to test all 26 possible alphabet rotations at once, look for normal English wording in the output, and double-check everything that the decoder gave me. I also made sure to write down the problems I ran into, like how shifting the whole wrapper string messed up the formatting. I fixed this by separating the platform wrapper from the actual hidden payload so they could be handled separately.


#3. Scope and Tools
Base64 Decoding Utility: A command-line string tool used to check any nested encoding layers and make sure the inner payload was clean.

Webshell: The command-line environment I used to grab files, move through directories, and run quick check commands.

DCode Caesar Cipher Tool: Automated brute-force tool used to test all 26 alphabet shifts at once so I could spot readable English text right away.



#4. Procedure Summary
Step 1: Grabbing the Ciphertext and Checking the File
Downloaded the challenge file from the CyLab dashboard and opened it up in a basic text viewer. I wanted to verify the file was intact, figure out what kind of cipher I was dealing with, and check for any wrapper formatting before jumping into decryption.

Step 2: Running Automated Brute-Force Shifts
Plugged the extracted ciphertext into a rotation tool to cycle through all 26 possible alphabet shifts. I scanned the resulting list for common English word patterns, trigrams, and recognizable syntax markers, which I did find.

Step 3: Secondary Base64 Decoding and Capturing the Flag
I realized that the inner decrypted output was still wrapped in another layer of Base64 encoding, so I knew I needed to decode it one more time before I could see the actual readable information. I opened up the terminal and used the clean command echo -n "encoded_string" | base64 --decode (omitting any prompt text to maintain technical accuracy) to decode the string and turn the byte stream into readable text. After that, I checked the output to make sure it was formatted correctly and matched the platform's expected format.


#5. Challenge Outcome
I was able to finish the challenge by working through the different possible shifts until I found the one that made the most sense. After figuring out the correct shift, I still had to decode the extra Base64 layer using the terminal before I could get to the final result. The part that really made everything click for me was realizing that investigations can have multiple layers hiding the actual information. In this case, it was a simple cipher combined with Base64 encoding, so I had to work through each layer instead of expecting everything to be readable right away. It showed me how even basic techniques can be combined to make information harder to recognize at first.


#6. Troubleshooting and Decision-Making
At first, I assumed that finding the correct Caesar shift offset would instantly give me the final plain text ready to submit for the flag. When my initial output string still looked scrambled and failed validation, a closer inspection showed that there was a secondary encoding layer hidden inside the cipher. That gave me the proof I needed to switch tactics: I took that intermediate string and ran it clean through the terminal using echo -n "encoded_string" | base64 --decode to unpack the underlying data, which finally got me the correct flag.


#7. Skills 
Classical Cryptanalysis
What I did: I tried different rotation shifts until I found a pattern that actually made sense and helped me figure out the correct key.  Career Relevance: Security Analyst uses this skill.

Command-Line Tooling
What I did: I used clean commands like base64 --decode, checked files, and used different string commands in a Linux webshell to figure out what was going on.
Career Relevance: Linux Admin or Incident Responder working with remote systems and checking files.

Multi-Layer DecodingWhat I did: I worked through multiple layers of encoding by using shift tools along with Base64 commands until I could get to the actual information.
Career Relevance: Forensic Investigator looking at modified files, system logs, or suspicious payloads.

Systematic TroubleshootingWhat I did: I figured out why my submissions were not working by checking the formatting, trying different shift keys, and going through each layer of the payload.
Career Relevance: SOC Analyst checking alerts, finding problems, and troubleshooting during security investigations.


#8. What I Learned
This lab was a really good reminder of why classical substitution ciphers just don't cut it for modern security, especially when people try to stack simple text encodings like Base64 on top of them. It really hammered home the idea that trying to hide things through mere obscurity isn't actual security. Tying this back into our broader data topics, it's easy to see why relying on weak shortcuts instead of robust, standardized algorithms leaves systems completely exposed.


#9. Career Connection
Working as a Security Operations Center (SOC) Analyst, you're going to constantly run into encoded strings, messy PowerShell scripts, or malicious command-line arguments that rely on basic shifts or Base64 encoding just to hide from detection rules. Writing up this walkthrough helps build those core analytical habits and makes working in a command-line terminal feel second nature, which translates directly to faster triage and better threat hunting during live incidents.

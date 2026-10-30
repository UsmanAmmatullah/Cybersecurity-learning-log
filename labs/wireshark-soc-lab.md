# Wireshark SOC Lab: Detecting a Port Scan

**Platform:** NetworkWalks Wireshark SOC Lab
**Date:** 30 October 2026
**Score on first attempt:** 60/100

## Scenario
An attacker targeted my company's web server. My job was to capture the traffic, analyse it in Wireshark, and find out what type of attack it was and what the attacker's IP address was. This was my first time ever using Wireshark.

## What I found
- The capture contained **309 packets**.
- Most of the traffic was normal: ARP, DNS and ordinary TCP connections.
- One IP address, **192.168.10.66**, sent a long block of connection requests (SYN packets) to the web server **192.168.10.20**.
- Each request went to a **different port**: **43 ports** in total.
- The server answered **3 ports as open** (SYN, ACK) and the rest as closed (RST).
- Conclusion: this is a **SYN port scan**, which attackers use to find out which services a server is running.

## How I did it
I used Wireshark's **display filter bar**. It is not a command line. It hides every packet that does not match, so you only see what you are asking about.

| Question | Filter I used |
| --- | --- |
| SYN-only packets | `tcp.flags.syn == 1 && tcp.flags.ack == 0` |
| Everything the attacker sent | `ip.src == 192.168.10.66` |
| Open ports (SYN, ACK) | `ip.src == 192.168.10.20 && ip.dst == 192.168.10.66 && tcp.flags.syn == 1 && tcp.flags.ack == 1` |
| Closed ports (RST) | `ip.src == 192.168.10.20 && ip.dst == 192.168.10.66 && tcp.flags.reset == 1` |

## Key ideas I learned
- A TCP connection starts with a three-way handshake: SYN, SYN-ACK, ACK.
- An open port replies SYN, ACK. A closed port replies RST.
- A scanner never completes the handshake.
- For "how many" questions, read the **Displayed** count in the status bar after filtering.

## Where I lost marks
I scored 60/100, so four tasks were marked wrong. I'm reviewing them.

## Next steps
- Fix the tasks I missed and retake the lab.
- Try the next Wireshark lab.

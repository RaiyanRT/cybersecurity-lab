[Back to Incident Response Plan](../incident-response-plan.md)

# DoS Attack Simulation - SYN flood

## Objective

Simulate a Denial of Service attack against a Windows 10 endpoint using hping3 from Kali Linux. Monitor the attack's impact on system resources, detect it using Wazuh SIEM and network monitoring tools, contain it using Windows Firewall rules, and document the full response using the NIST Incident Response Framework.

## Environment

| Machine | OS | Role |
| --- | --- | --- |
| Kali Linux | Kali | Attacker — launches SYN flood |
| Windows 10 | Windows | Target endpoint — Wazuh agent installed |
| Wazuh Server | Linux | SIEM — monitors for unusual network activity |

## What is a SYN Flood?

A SYN flood exploits the TCP three-way handshake. Normally, a connection works like this:

1. Client sends a **SYN** (synchronise) packet to the server
2. Server responds with a **SYN-ACK** (synchronise-acknowledge)
3. Client sends an **ACK** (acknowledge) to complete the connection


In a SYN flood, the attacker sends thousands of SYN packets but never completes the handshake — they never send the final ACK. The server keeps each half-open connection in memory, waiting for the ACK that never comes. Eventually, the server's connection table fills up and it can no longer accept legitimate connections.

This is one of the simplest and most common denial of service attacks.

## Runbook (Red Team)

Before launching the attack, record the normal state of the Windows 10 target so the impact can be measured.

**On the Windows 10 target:**
- Open Task Manager → Performance tab. Note the current CPU usage and network activity.
- Open Wireshark and start a capture on the active network interface. Let it run for 30 seconds to establish a baseline of normal traffic.

![Task Manager CPU and Network Usage](assets/dos-1.png)

Currently the device is settled at around a 46% usage and the network usage is around

![WireShark Showing Current Network Packets](assets/dos-2.png)

On Wireshark you can see that currently the only packets that are travelling through and from the windows system is the normal TCP three-way handshake with the Wazuh server, sending logs and messages to port 1514.

### Step 2: Launch the SYN Flood

On the Kali attacker, run:

```bash
hping3 -S --flood -V -p 80 <windows-10-ip>
```



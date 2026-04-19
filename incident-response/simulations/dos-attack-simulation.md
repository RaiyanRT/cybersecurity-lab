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

Currently the device is settled at around a 3% usage and the network usage is not very high at all with the occastional communication with the wazuh server.

![WireShark Showing Current Network Packets](assets/dos-2.png)

On Wireshark you can see that currently the only packets that are travelling through and from the windows system is the normal TCP three-way handshake with the Wazuh server, sending logs and messages to port 1514.

### Step 2: Launch the SYN Flood

On the Kali attacker, run:

```bash
hping3 -S --flood -V -p 80 <target-ip>
```

**What each flag does:**

`-S` — Sets the SYN flag on every packet (initiating a TCP handshake that will never complete)

`--flood` — Sends packets as fast as possible without waiting for replies

`-V` — Verbose output so we can see what's being sent

`-p 80` — Targets port 80 (HTTP). This could be any open port.

### Step 3: Observe the Impact

**On the Windows 10 target:**

While the attack is running, check Task Manager again. CPU usage and network activity should spike significantly compared to the baseline.

Switch to Wireshark — the capture should now show a flood of SYN packets all originating from the Kali attacker's IP address.

![CPU and Network usage after the attack](assets/dos-3.png)

As you can see CPU usage spiked all the way to 100% while ethernet 2 (host-only network) showed 29.3 Mbps of received packets.

![WireSHark Showing the large amount of TCP requests](assets/dos-4.png)

From Wireshark you can notice the large amount of TCP packets being sent from the IP of the Kali attacker.

### Step 4: Stop the Attack

Press `Ctrl+C` on the Kali terminal to stop hping3. Note the total number of packets sent (hping3 displays this on exit).

![The number of packets sent](assets/dos-5.png)

As you can see 50156393 packets were sent to the windows 10 machine. 

### Phase 2: Detection & Analysis
 
**How would this be detected in a real environment?**
 
In a production environment, a DoS attack might be noticed through:
- Users reporting that services are slow or unreachable
- Network monitoring tools alerting on unusual bandwidth spikes
- SIEM alerts triggered by a high volume of connection attempts from a single source
- System performance degradation visible in monitoring dashboards
**In this lab:**
 
**Wireshark analysis:**
 
Filter the Wireshark capture to show only SYN packets from the attacker:
 
```
tcp.flags.syn == 1 && ip.src == <kali-ip>
```
 
This should show thousands of SYN packets with no corresponding ACK completions — a clear signature of a SYN flood.




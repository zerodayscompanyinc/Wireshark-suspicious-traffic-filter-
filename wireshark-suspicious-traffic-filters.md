# Wireshark 01: Display Filters for Spotting Suspicious Traffic

Wireshark is used to analyze network traffic. These display filters help you find five kinds of suspicious activity:

- Data exfiltration
- Hacking tools
- Network mapping
- Command and control
- PsExec attacks

---

## 1. Data Exfiltration

**Filter:**
```
dns.qry.name.len > 30
```

**Field:** `dns.qry.name.len` (the length of the DNS query name)

**What it finds:** DNS queries with very long domain names, longer than 30 characters. Names this long can point to DNS tunneling, where stolen data is hidden inside DNS requests and sent out of the network.

---

## 2. Hacking Tools

**Filter:**
```
http.user_agent contains "curl"
```

**Field:** `http.user_agent`

**What it finds:** HTTP requests sent by the curl command-line tool. Regular users don't browse with curl, so it can be a sign that someone is using a tool instead of a normal browser.

---

## 3. Network Mapping

**Filter:**
```
tcp.flags.syn == 1 && tcp.flags.ack == 0
```

**Field:** `tcp.flags.syn` and `tcp.flags.ack`

**What it finds:** TCP packets with the SYN flag on and the ACK flag off. These are connection-start packets with no reply attached. A lot of them coming from one source can mean someone is scanning your network to see what is there.

---

## 4. Command and Control (C2)

**Filter:**
```
dns.qry.type == 16
```

**Field:** `dns.qry.type` (type 16 is a TXT record)

**What it finds:** DNS TXT record requests. Malware can use TXT records to receive instructions from its operator, so these can reveal hidden C2 communication.

---

## 5. PsExec Attacks

**Filter:**
```
frame contains "PSEXESVC"
```

**Field:** `frame`

**What it finds:** Any packet containing the text "PSEXESVC", which is the service name PsExec creates on a remote machine. Seeing it can mean an attacker is moving from one system to another (lateral movement) and a PsExec attack is in progress.

---

## Quick Reference

| # | What to look for | Filter |
|---|---|---|
| 1 | Data exfiltration | `dns.qry.name.len > 30` |
| 2 | Hacking tools | `http.user_agent contains "curl"` |
| 3 | Network mapping | `tcp.flags.syn == 1 && tcp.flags.ack == 0` |
| 4 | Command and control | `dns.qry.type == 16` |
| 5 | PsExec attacks | `frame contains "PSEXESVC"` |

*Source: video by Bbck2basics on Wireshark traffic analysis.*

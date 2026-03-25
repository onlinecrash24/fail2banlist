# 🛡️ Public IP Blocklist (Fail2Ban Sync)

This repository provides a real-time synchronized list of malicious IP addresses detected and blocked by automated security systems. 

The list is primarily focused on protecting network services from brute-force attacks, unauthorized relay attempts, and protocol-level scanning.

## 📊 Monitored Activities
IPs are added to this list based on the following triggers:
- **SSH Brute-force:** Repeated unauthorized login attempts.
- **SMTP/Mail Scanners:** Bots attempting improper command pipelining or malformed requests.
- **Relay Attempts:** Unauthorized attempts to use mail services as an open relay.
- **Authentication Failures:** Failed SASL/SMTP login attempts.

## 📂 Data Format
- **`banlist.txt`**: A plain-text file containing unique, sorted IP addresses (one per line).

## 🚀 Usage
You can use this list to pre-emptively secure your own environment by integrating it into your firewall or security tools.

### Example: Block via IPSet (Linux)
```bash
# Create an ipset
ipset create global-blacklist hash:net

# Download and add IPs
curl -s https://raw.githubusercontent.com/onlinecrash24/fail2banlist/main/banlist.txt | xargs -I {} ipset add global-blacklist {}

# Block via iptables
iptables -I INPUT -m set --match-set global-blacklist src -j DROP

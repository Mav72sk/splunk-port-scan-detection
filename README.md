# Port Scan Detection with Splunk

This mini‑SIEM use‑case simulates and detects TCP port‑scanning activity—a common reconnaissance tactic (MITRE ATT&CK T1046).

Use‑case 
Identify any source IP that connects to three or more distinct destination ports within a short window.
 Tools Used 
- **Splunk Enterprise** (free 60‑day trial, macOS tgz + Rosetta)
- Sample firewall/syslog events (`port_scan_logs.txt`)
- SPL (Search Processing Language)

Quick Start 
1. **Add Data** → upload `port_scan_logs.txt` to index `port_scan_demo` (source type `syslog`). 
2. Run: Search Query (SPL) used: 
   ```spl
   index=port_scan_demo "Drop TCP connection attempt"
   | rex "from (?<src_ip>\\d{1,3}(?:\\.\\d{1,3}){3}) to port (?<dest_port>\\d+)"
   | stats dc(dest_port) AS unique_ports by src_ip
   | where unique_ports >= 3

## Output
| src_ip        | unique_ports |
|---------------|--------------|
| 192.168.1.100 | 4            |

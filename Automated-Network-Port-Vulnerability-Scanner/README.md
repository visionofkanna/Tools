# Automated Network & Port Vulnerability Scanner

A single tool that replaces the separate **Network Scanner** and **Port Scanner**
projects. It discovers live hosts, probes their ports, fingerprints services from
banners, and flags likely vulnerabilities and risky exposures, with a GUI, a CLI and
exportable reports.

> **Use responsibly.** Only scan systems you own or have explicit written permission
> to test. The GUI requires an authorisation checkbox and the CLI asks for
> confirmation (or `--yes`).

## Pipeline

```
 targets ──► host discovery ──► port scan (TCP + optional UDP) ──► banner grab
 IP/CIDR/    ARP (scapy)         threaded, per-host                 HTTP/TLS aware
 hostname    or TCP fallback
                                                  │
                  JSON · HTML · CSV · TXT ◄── findings ◄── vulnerability rules
```

| Stage | Origin | What it does now |
|---|---|---|
| Discovery | Network Scanner | ARP sweep via scapy; falls back to TCP probing when ARP is unavailable (no scapy, no admin rights, or a routed network) |
| Port scan | Port Scanner | Threaded TCP connect scan, protocol-aware banner grabbing, UDP probes for DNS/NTP/SNMP |
| Analysis | **new** | Offline rules: risky exposed services, version-based CVE hints, obsolete protocols, version disclosure |
| Reporting | **new** | JSON, HTML, CSV, plain text; severity-based CLI exit code for CI use |

## Install

```bash
git clone <your-repo-url>
cd Automated-Network-Port-Vulnerability-Scanner
pip install -r requirements.txt        # scapy is optional (ARP discovery only)
```

Python 3.9+. The GUI needs Tkinter (bundled with most Python installs). On Windows,
ARP discovery also needs [Npcap](https://npcap.com) and an administrator shell; on
Linux/macOS run with `sudo`.

## Usage

```bash
python -m netvulnscan                          # no arguments: opens the GUI
python -m netvulnscan --local                  # scan your local /24
python -m netvulnscan 192.168.1.0/24 -p web    # discovery + web ports
python -m netvulnscan 10.0.0.5 -p 1-1024 --udp -o reports/
python -m netvulnscan scanme.example.org -p 22,80,8000-8100 --fail-on high -y
```

| Option | Meaning |
|---|---|
| `-p, --ports` | `quick` (top ~50), `web`, `mail`, `full` (1-1024) or a list like `22,80,8000-8100` |
| `--udp` | also probe UDP DNS (53), NTP (123) and SNMP (161) |
| `--discovery` | `auto` (default), `arp`, `tcp`, or `none` for multi-host targets |
| `--workers`, `--timeout` | concurrency and per-port timeout |
| `--max-hosts` | safety cap on target size (default 1024) |
| `-o, --output-dir`, `--format` | write `json,html,csv,txt` reports |
| `--fail-on SEVERITY` | exit status 2 if a finding at/above that level exists |

## What it detects

- **Risky exposure**: Telnet, FTP, SMB, RDP, VNC, r-services, exposed databases and
  caches (MySQL, PostgreSQL, MSSQL, Redis, MongoDB, Memcached, Elasticsearch), the Docker API, SNMP with the `public` community.
- **Version-based hints** (from banners): vsftpd 2.3.4 backdoor, ProFTPD 1.3.5 mod_copy,
  Apache 2.4.49/2.4.50 path traversal, EOL Apache 2.2 and IIS 6, nginx and OpenSSH version ranges.
- **Hygiene**: SSH protocol 1, software versions disclosed in HTTP headers.

Rules live in `netvulnscan/vulndb.py` and are easy to extend.

## Limitations (read before trusting a result)

- Findings are **indicative**: they come from open ports and banner strings, not from
  exploitation. Backported patches and hidden banners cause false positives and negatives, so verify manually.
- IPv4 only. ARP discovery covers the local segment; the "my network" button assumes a /24.
- UDP: only hosts that answer are reported; silence cannot distinguish open from filtered.
- The rule set is small and offline by design; it is not a substitute for Nessus, OpenVAS or Nmap.

## Project layout

```
netvulnscan/
  discovery.py   ARP sweep + TCP fallback
  portscan.py    TCP/UDP probing, banner grabbing, port-spec parser
  vulndb.py      vulnerability rules
  engine.py      orchestration (used by both CLI and GUI)
  report.py      text / JSON / CSV / HTML output
  cli.py  gui.py  __main__.py
tests/           unittest suite incl. a localhost end-to-end scan
```

Run the tests with `python -m unittest discover -s tests -v`.

## Improvements over the original tools

- Scanning runs on worker threads; the old Port Scanner froze the GUI during scans and
  the Network Scanner updated Tk widgets from a background thread.
- Closed ports are no longer dumped to the results file; only open ports and findings are reported.
- HTTP/HTTPS banners are actually retrieved (a bare `recv` returns nothing from web servers).
- The "detected OS" line described the *local* machine, not the target, so it was dropped.
- Input validation, target-size cap, and no bare `except:` clauses.

---
Crafted by Rakesh Kanna

#  Subdomain Scanner

A fast, wordlist-based **subdomain enumeration tool** for Windows. Give it a target domain, and it checks thousands of common subdomain names to discover which ones actually exist, helping you map out the attack surface of a domain during reconnaissance.

Ships as a ready-to-run **`SubdomainScanner.exe`** (no Python installation required) along with a bundled wordlist of the **top 5,000 most common subdomains**.

> **For authorized security testing and educational purposes only.** See the [Disclaimer](#️-disclaimer).

---

##  Features

-  Simple desktop GUI, no command line needed
-  Bundled wordlist with 5,000 common subdomains (`www`, `mail`, `ftp`, `webmail`, ...)
-  Brute-force enumeration by resolving `<word>.<target-domain>`
-  Support for custom wordlists (plain `.txt`, one subdomain per line)
-  Standalone Windows executable, just download and run

---

##  Repository Structure

```
Subdomain-Scanner/
├── SubdomainScanner.exe              # Standalone Windows application
├── subdomains-top1million-5000.txt   # Wordlist (top 5,000 subdomains)
└── README.md
```

---

##  Getting Started

### Requirements

- Windows 10 / 11
- Internet connection (required to resolve DNS records)

### Installation

1. Clone the repository or download it as a ZIP:
   ```bash
   git clone https://github.com/<your-username>/<your-repo>.git
   ```
2. Keep `SubdomainScanner.exe` and `subdomains-top1million-5000.txt` together in the same folder.
3. Double-click **`SubdomainScanner.exe`** to launch.

>  If Windows SmartScreen shows a warning, click **More info → Run anyway**. This happens because the executable is unsigned.

---

##  Usage

1. Launch `SubdomainScanner.exe`.
2. Enter the **target domain** (e.g. `example.com`).
3. Select the wordlist file (`subdomains-top1million-5000.txt`) or your own.
4. Click **Start Scan**.
5. Discovered subdomains appear in the results list as they are found.

### Example

| Input         | Checked subdomains                                        |
|---------------|-----------------------------------------------------------|
| `example.com` | `www.example.com`, `mail.example.com`, `ftp.example.com`, … |

---

##  How It Works

1. Reads each entry from the wordlist.
2. Builds a candidate hostname: `<entry>.<domain>`.
3. Attempts to resolve the hostname via DNS.
4. Subdomains that resolve successfully are reported as **valid**; the rest are discarded.

---
##  Build From Source

The executable was built from a Python application. To package it yourself:

```bash
pip install pyinstaller
pyinstaller --onefile --windowed SubdomainScanner.py
```

The compiled file will be in the `dist/` folder.

---

##  Wordlist

`subdomains-top1million-5000.txt` contains the 5,000 most common subdomain names, sourced from the **top 1 million** subdomain lists popularly used in security research (e.g. [SecLists](https://github.com/danielmiessler/SecLists)). You can swap in any larger wordlist for deeper scans.

---

##  Roadmap

- [ ] Export results to TXT / CSV
- [ ] Multi-threaded scanning controls
- [ ] Progress bar and scan statistics
- [ ] HTTP status / title detection for found subdomains

---

##  Author

**Rakesh Kanna**

If you found this useful, consider giving the repo a star!

---


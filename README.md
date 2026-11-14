#  Python Cybersecurity Projects

These projects cover network reconnaissance, PDF security, and domain enumeration, and are intended for learning, practice, and authorized security testing.


---

##  Projects Overview

| # | Project | Description |
|---|---------|-------------|
| 1 | [Network Scanner](./Network-Scanner) | Discovers live hosts on a local network and lists their IP and MAC addresses. |
| 2 | [PDF Cracker Tool](./PDF-Cracker-Tool) | Recovers the password of a protected PDF using a wordlist (dictionary) attack. |
| 3 | [PDF Protection Tool](./PDF-Protection-Tool) | Encrypts PDF files with a password to keep documents secure. |
| 4 | [Port Scanner](./Port-Scanner) | Scans a target host for open TCP ports and identifies common services. |
| 5 | [Subdomain Enumeration Tool](./Subdomain-Enumeration-Tool) | Finds valid subdomains of a domain using a wordlist. |

---

### 1.  Network Scanner
Scans a network range to identify active devices.
- Detects live hosts on a subnet
- Displays IP and MAC addresses
- Useful for network mapping and inventory

### 2.  PDF Cracker Tool
Recovers forgotten passwords from **your own** password-protected PDFs.
- Wordlist / dictionary-based password recovery
- Shows progress and the recovered password
- Demonstrates why weak passwords are risky

### 3.  PDF Protection Tool
Adds password protection to PDF documents.
- Encrypts a PDF with a user-defined password
- Outputs a new protected file
- Simple command-line usage

### 4.  Port Scanner
Checks which ports are open on a target machine.
- Scans a single port, a range, or common ports
- Identifies well-known services (HTTP, SSH, FTP, etc.)
- Supports multithreading for faster scans

### 5.  Subdomain Enumeration Tool
Discovers subdomains associated with a target domain.
- Wordlist-based brute-force enumeration
- Reports valid and resolving subdomains
- Helpful for reconnaissance in authorized testing

---

##  Repository Structure

```
.
├── Network-Scanner/
│   ├── network_scanner.py
│   └── README.md
├── PDF-Cracker-Tool/
│   ├── pdf_cracker.py
│   ├── wordlist.txt
│   └── README.md
├── PDF-Protection-Tool/
│   ├── pdf_protect.py
│   └── README.md
├── Port-Scanner/
│   ├── port_scanner.py
│   └── README.md
├── Subdomain-Enumeration-Tool/
│   ├── subdomain_enum.py
│   ├── subdomains.txt
│   └── README.md
├── requirements.txt
└── README.md
```

> Adjust folder and file names to match your actual repository.

---

##  Getting Started

### Prerequisites
- Python 3.8 or higher
- `pip` package manager
- Git

### Installation

```bash
# Clone the repository
git clone https://github.com/<your-username>/<your-repo-name>.git

# Move into the project folder
cd <your-repo-name>

# (Optional) Create a virtual environment
python -m venv venv
source venv/bin/activate      # Linux / macOS
venv\Scripts\activate         # Windows

# Install dependencies
pip install -r requirements.txt
```

---

## Usage

Run each tool from its own folder. Example commands (update to match your scripts):

```bash
# Network Scanner
python network_scanner.py -t 192.168.1.0/24

# PDF Cracker (own files only)
python pdf_cracker.py -f protected.pdf -w wordlist.txt

# PDF Protection
python pdf_protect.py -i input.pdf -o secured.pdf -p yourpassword

# Port Scanner
python port_scanner.py -t 192.168.1.10 -p 1-1024

# Subdomain Enumeration
python subdomain_enum.py -d example.com -w subdomains.txt
```

---

## Technologies Used

- **Python 3**
- `socket`, `threading`, `argparse` (standard library)
- `scapy` – network scanning
- `pikepdf` / `PyPDF2` – PDF handling
- `requests` / `dnspython` – subdomain enumeration

---

## Legal Disclaimer

These tools are created **for educational purposes and authorized security testing only**.

- Only scan networks, hosts, and domains that you **own** or have **explicit written permission** to test.
- Only use the PDF tools on files that belong to you.
- Unauthorized scanning, access, or password cracking may be **illegal** in your jurisdiction.
- The author accepts **no responsibility** for misuse or damage caused by these tools.

---

## Contributing

Contributions are welcome!

1. Fork the repository
2. Create a feature branch: `git checkout -b feature/your-feature`
3. Commit your changes: `git commit -m "Add your feature"`
4. Push to the branch: `git push origin feature/your-feature`
5. Open a Pull Request

---

## License

This project is licensed under the [MIT License](./LICENSE).

---


If you find this repository useful, consider giving it a star!

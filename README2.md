# Automated Port Scanner for Network Reconnaissance

A Python-based tool for ethical port scanning using Nmap, with service detection, vulnerability reporting, and multi-format reports.

## Table of Contents
- [Introduction and Objective](#introduction-and-objective)
- [Key Features](#key-features)
- [Technologies and Dependencies](#technologies-and-dependencies)
- [Architecture and Code Structure](#architecture-and-code-structure)
- [Installation and Usage Instructions](#installation-and-usage-instructions)
- [Usage Examples and Testing](#usage-examples-and-testing)
- [Testing and Validation](#testing-and-validation)
- [Conclusion](#conclusion)

## Introduction and Objective
This project involves developing an automated tool in Python to scan open ports on a network target, integrating the Nmap tool for ethical reconnaissance. The main objective is educational: to allow beginners in cybersecurity and Python programming to learn the basics of network reconnaissance while creating a practical tool to detect active services, identify potential vulnerabilities, and generate structured reports. The project emphasizes ethics – it should only be used on networks you control or with explicit permission, to avoid any illegal activities like unauthorized hacking.

The scanner is designed to be simple, extensible, and tested on virtual environments like Kali Linux, focusing on non-intrusive scans. It draws inspiration from tools like Nmap but provides a customizable Python interface.

## Key Features
- **Port Scanning**: By default, scans common ports (22/SSH, 80/HTTP, 443/HTTPS). Ability to customize via arguments or a file (e.g., add 21/FTP or 25/SMTP).
- **Service Detection**: Uses Nmap to identify active services, their versions, and the target's operating system.
- **Vulnerability Reporting**: Compares detected services to a simple database of known vulnerabilities (based on public CVEs). Alerts on potential flaws (e.g., outdated SSH versions).
- **Reports**: Automatic generation of reports in three formats:
  - **TXT**: Readable text report for a quick overview.
  - **CSV**: Tabular format for analysis in tools like Excel.
  - **JSON**: Structured format for integration with other programs or APIs.
- **Automation and Interface**: Launched via command line with arguments (e.g., `--target IP --ports 22,80,443`). Includes error handling and ethical warnings.
- **Implemented Improvements**:
  - **Customizable Ports**: Users can specify ports via a `--ports` argument or a `--ports-file`.
  - **Multithreading**: Uses threads to scan multiple ports in parallel, speeding up the process without overloading the network.
  - **Logging**: Logs all actions (scans, errors, results) in a file for debugging and auditing.

## Technologies and Dependencies
- **Language**: Python 3.x (recommended for its simplicity and libraries).
- **Key Libraries**:
  - `python-nmap`: To integrate Nmap and perform advanced scans.
  - `argparse`: To handle command-line arguments.
  - `threading`: For multithreading scans.
  - `logging`: For logging.
  - `csv` and `json`: For generating reports.
  - `socket`: For basic checks (optional but useful for simple tests).
- **External Tools**: Nmap must be installed on the system (available by default on Kali Linux via `sudo apt install nmap`).
- **Testing Environment**: Kali Linux VMs (for the scanner) and a target like Metasploitable (to simulate vulnerabilities).

## Architecture and Code Structure
The code is organized into a single script (`port_scanner.py`) for simplicity. Here's the logical structure:
- **Imports and Configuration**: Loading libraries and setting up logging.
- **Scan Function**: A main function to scan a port with Nmap, using threads for parallelization.
- **Vulnerability Detection**: A simple function to check known CVEs (based on a hardcoded list to keep it lightweight; extensible to a real API).
- **Report Generation**: Functions to create TXT, CSV, and JSON files.
- **User Interface**: Parsing arguments and launching the scan with an ethical warning.
- **Error Handling**: Try-except to catch network or permission issues.

The code is modular, with humanized comments to explain each part as if chatting with a friend.

## Installation and Usage Instructions
1. **Installation**:
   - Install Python 3.x if not already done.
   - Install Nmap: `sudo apt install nmap` (on Linux/Kali).
   - Create a virtual environment: `python3 -m venv scanner_env` then `source scanner_env/bin/activate`.
   - Install dependencies: `pip install python-nmap`.

2. **Usage**:
   - Run the script: `python port_scanner.py --target <IP> --ports 22,80,443` (example for a local IP).
   - Options:
     - `--target`: Target IP or domain (required).
     - `--ports`: Comma-separated list of ports (optional, default: 22,80,443).
     - `--ports-file`: Path to a text file with one port per line (optional).
     - `--output-dir`: Folder for reports (default: `./reports`).
   - Examples:
     - Basic scan: `python port_scanner.py --target 192.168.1.1`
     - With custom ports: `python port_scanner.py --target 192.168.1.1 --ports 21,22,80,443`
     - With file: Create `ports.txt` with "21\n22\n80\n443", then `python port_scanner.py --target 192.168.1.1 --ports-file ports.txt`

3. **Output**:
   - Results displayed in the console.
   - Reports generated in `./reports/` (e.g., `scan_report.txt`, `scan_report.csv`, `scan_report.json`).
   - Logs in `scanner.log`.

## Usage Examples and Testing
- **Test on Localhost**: `python port_scanner.py --target 127.0.0.1` – Scans local ports to verify functionality.
- **Test on VM**: On Kali, target a Metasploitable VM (IP: 192.168.56.101). This will reveal vulnerable services like an old FTP server.
- **Example Result**: For an open port (e.g., 80 with Apache), the TXT report might say: "Port 80 open: HTTP Service (Apache 2.2.8) – Detected Vulnerability: CVE-2011-3192 (Possible DoS)."
- **Ethics**: Always test on your own machines. The script displays a warning: "Warning: Use this tool only on authorized networks. Unauthorized scanning is illegal."

## Testing and Validation
- **Unit Tests**: The code includes basic checks (e.g., if Nmap is installed). Test with invalid IPs to see errors.
- **Performance**: Multithreading reduces scan time (e.g., 10 ports in ~5 seconds vs. 30 seconds sequentially).
- **Limitations**: Does not replace full Nmap; it's an educational wrapper. For advanced scans, use Nmap directly.
- **Debugging**: Check `scanner.log` for errors (e.g., "Error: Unable to scan port 80 – Connection refused").

## Conclusion
This project is an excellent introduction to Python and cybersecurity, combining programming, networking, and ethics. With the improvements (customizable ports, multithreading, logging), it remains beginner-friendly while being extensible. If you use it to learn, share your feedback! For future evolutions, we could add a GUI or API integration for real-time CVEs.

Feel free to contribute or report issues!

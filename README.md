# WebInsight Scout - DAST Vulnerability Scanner

A conceptual **Dynamic Application Security Testing (DAST)** tool implemented in Python, designed to illustrate fundamental DAST principles. It interacts with live web applications by sending HTTP GET requests and analyzing the responses for potential security vulnerabilities and information disclosure.

## Overview

WebInsight Scout is an educational tool that demonstrates how DAST tools operate at a conceptual level. It performs passive security analysis on web applications without injecting malicious payloads or active exploitation.

## Key Features

### 🔍 Basic Web Interaction
- Uses the `requests` library to make HTTP requests to target URLs
- Checks HTTP status codes and response headers
- Handles connection timeouts and network errors gracefully

### 📄 HTML Content Parsing
Employs BeautifulSoup to parse HTML responses for:

#### Link and Form Discovery
- Identifies and counts all `<a>` tags (links)
- Identifies and counts all `<form>` tags
- Detects suspicious link attributes (`javascript:`, `data:`)
- Flags forms with password fields using GET method

#### HTML Comment Analysis
- Scans HTML comments for potentially sensitive information
- Detects keywords: `password`, `api_key`, `secret`, `admin`, `token`
- Reports findings with medium severity

### 🔎 Keyword-Based Vulnerability Detection
Analyzes response body for security indicators:

| Keyword | Detected Vulnerability | Severity |
|---------|----------------------|----------|
| `error`, `exception` | Information Disclosure | Medium |
| `stack trace` | Information Disclosure | Medium |
| `database`, `sql`, `query` | SQL Injection / Info Disclosure | High/Medium |
| `javascript error` | Client-side Vulnerability | Medium |

### 🛡️ Robust Error Handling
- Connection timeouts with configurable timeout periods
- Connection error handling
- General exception handling with informative error messages
- Graceful degradation on failures

### 📊 Simplified Reporting
- Collects all findings with type, severity, and description
- Groups findings by severity (High, Medium, Low)
- Provides summary statistics
- Professional output formatting

## Installation

### Requirements
- Python 3.7+
- `requests` library
- `beautifulsoup4` library

### Setup
```bash
pip install -r requirements.txt
```

Or install manually:
```bash
pip install requests beautifulsoup4
```

## Usage

### Command Line
```bash
python DAST_VULNERABILITY_SCANNER.py <target_url>
```

### Examples
```bash
# Scan with HTTPS
python DAST_VULNERABILITY_SCANNER.py https://example.com

# Scan with HTTP
python DAST_VULNERABILITY_SCANNER.py http://example.com

# URL scheme added automatically if not provided
python DAST_VULNERABILITY_SCANNER.py example.com
```

### Output Example
```
[*] Starting DAST scan on: https://example.com
[*] ============================================================
[*] Fetching URL: https://example.com
[+] HTTP Status Code: 200
[*] Analyzing HTTP response...
[*] Parsing HTML content...
[+] Found 42 links
[+] Found 3 forms

============================================================
[*] SCAN REPORT
============================================================
[!] Total findings: 2

HIGH SEVERITY:
  • [HIGH] Insecure_Form: Form with password field using GET method - credentials may be logged (URL: https://example.com)

MEDIUM SEVERITY:
  • [MEDIUM] information_disclosure: Detected 'error' in response body - potential information disclosure (URL: https://example.com)

============================================================
IMPORTANT LIMITATIONS:
============================================================
✗ This is a conceptual/educational tool only
✗ No payload injection or active testing
✗ No web crawling or session management
✗ Simple pattern matching (high false positive rate)
✗ No contextual or business logic analysis
✗ NOT suitable for production security audits
============================================================
```

## Project Structure

```
DAST-tool-PyCodeScan/
├── DAST_VULNERABILITY_SCANNER.py    # Main scanner implementation
├── README.md                         # This file
├── requirements.txt                  # Python dependencies
└── PORTFOLIO_ENTRY.md               # Portfolio documentation
```

## Important Limitations ⚠️

**This is NOT a production-ready DAST tool.** It has significant limitations:

### ❌ Conceptual Only
- Serves purely for **educational purposes** to demonstrate DAST concepts
- Not intended for actual security auditing of real systems
- High potential for both false positives and false negatives

### ❌ No Payload Injection
- Does **NOT** inject malicious payloads to actively test for vulnerabilities
- Cannot detect SQL Injection, XSS, Command Injection, or other input-based vulnerabilities through testing
- Performs passive analysis only based on response patterns

### ❌ No Web Crawling or State Management
- Processes only a **single, explicitly provided URL** at a time
- Lacks ability to autonomously discover application paths
- No session management or authentication handling
- No support for multi-step interactions or stateful testing

### ❌ Simple Pattern Matching
- Vulnerability detection relies on basic keyword matching
- High potential for **false positives** (e.g., detecting "stack trace" on google.com)
- High potential for **false negatives** (missing sophisticated vulnerabilities)
- No understanding of context or data flow

### ❌ No Contextual Analysis
- Does not understand application's business logic
- No control flow or data flow analysis
- Cannot identify complex vulnerabilities requiring contextual understanding
- No behavioral analysis

## Educational Value

This tool is designed to teach:
- ✓ Basic HTTP protocol concepts
- ✓ HTML parsing and analysis fundamentals
- ✓ Simple pattern matching for security indicators
- ✓ Error handling and resilient code practices
- ✓ DAST methodology at a conceptual level
- ✓ Limitations of passive security scanning

## Code Architecture

### Main Classes

#### `Finding`
Represents a single security finding with:
- Type (e.g., "SQL_Injection", "Information_Disclosure")
- Severity level (high, medium, low)
- Description
- URL where found

#### `DASTVulnerabilityScanner`
Main scanner class with methods for:
- `scan()` - Main entry point for scanning
- `_fetch_url()` - HTTP request and basic analysis
- `_analyze_http_response()` - Response body analysis
- `_analyze_html_content()` - HTML parsing and element detection
- `_analyze_comments()` - Sensitive comment detection
- `_generate_report()` - Report generation and formatting

### Key Features

**Type Hints**: Full type annotations for better code clarity and IDE support

**Comprehensive Docstrings**: Every class and method is documented with purpose, parameters, and return values

**Modular Design**: Separated concerns for different types of analysis

**Error Handling**: Graceful handling of network issues and parsing errors

**Object-Oriented**: Clean separation of concerns with the Finding class for data representation

## Comparison to Real DAST Tools

Real DAST tools like Burp Suite, OWASP ZAP, and Acunetix provide:
- Active payload injection and testing
- Web crawling and recursive discovery
- Session and state management
- Authentication handling
- Complex vulnerability detection algorithms
- Context-aware analysis
- Production-grade reporting

## Security Disclaimer

⚠️ **This tool should only be used on systems you have explicit permission to test.** Unauthorized security testing is illegal and unethical.

## Future Improvement Ideas (Educational Exercises)

Potential enhancements for learning:
1. Add support for multiple URLs with basic crawling
2. Implement simple payload injection for XSS testing
3. Add basic HTTP authentication support
4. Create more sophisticated detection patterns
5. Add export capabilities (JSON, HTML reports)
6. Implement multi-threading for concurrent scanning
7. Add command-line argument parsing for customization
8. Create configuration file support

## Getting Started

1. **Clone or download** this repository
2. **Install dependencies**: `pip install -r requirements.txt`
3. **Run a scan**: `python DAST_VULNERABILITY_SCANNER.py https://example.com`
4. **Review the output** and understand the limitations

## Example Workflow

```python
from DAST_VULNERABILITY_SCANNER import DASTVulnerabilityScanner

# Create scanner instance
scanner = DASTVulnerabilityScanner(timeout=15)

# Perform scan
report = scanner.scan("https://example.com")

# Access results
print(f"Total findings: {report['total_findings']}")
print(f"High severity: {report['findings_by_severity']['high']}")
print(f"Medium severity: {report['findings_by_severity']['medium']}")
print(f"Low severity: {report['findings_by_severity']['low']}")

# Iterate through findings
for finding in report['findings']:
    print(f"{finding.severity}: {finding.type} - {finding.description}")
```

## Learning Outcomes

By studying this code, you'll learn:
- HTTP protocol basics and request/response handling
- HTML parsing with BeautifulSoup
- Exception handling and resilient code design
- Object-oriented Python programming
- Security scanning concepts at a conceptual level
- Professional documentation practices
- Code organization and modularity

## License

This project is for educational purposes. Use responsibly and only on systems you have permission to test.

## Author

**mactrevors349-afk**

---

**Disclaimer**: This tool is provided for educational and authorized security testing only. Do not use it for unauthorized access or testing. The author is not responsible for any misuse or damage caused by this tool.

## Related Resources

- [OWASP Testing Guide](https://owasp.org/www-project-web-security-testing-guide/)
- [DAST Concepts](https://owasp.org/www-community/attacks/)
- [BeautifulSoup Documentation](https://www.crummy.com/software/BeautifulSoup/bs4/doc/)
- [Requests Library Documentation](https://docs.python-requests.org/)

---

**Last Updated**: July 1, 2026

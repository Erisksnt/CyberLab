## Relatório Técnico - Metasploitable & Medusa Lab

### 1. Objetivo
Descrever os testes de força bruta realizados entre Kali Linux e Metasploitable 2, utilizando a ferramenta Medusa e ambientes vulneráveis como DVWA.

### 2. Ambiente de Teste
- **Host:** Windows 10 + VirtualBox
- **VM1 (Atacante):** Kali Linux
- **VM2 (Alvo):** Metasploitable 2
- **Rede:** Host-Only (isolada, sem internet)
- **Endereços IP:**
  - Kali: 192.168.56.101
  - Metasploitable: 192.168.56.102

### 3. Ferramentas Utilizadas
- Medusa
- Nmap
- DVWA
- SMBclient / enum4linux

### 4. Cenários Testados
1. Ataque de força bruta em FTP  
2. Automação de tentativas em formulário DVWA  
3. Password spraying em SMB

### 5. Comandos e Execução
(Capturas, prints, ou links para os arquivos em \`exploits-and-commands/\`)

### 6. Resultados
(Descrever resultados obtidos, senhas descobertas, tempo médio, número de tentativas, etc.)

### 7. Mitigação
- Políticas de bloqueio após tentativas falhas consecutivas  
- Uso de autenticação multifator  
- Senhas complexas e expiração periódica  
- Monitoramento de logs e alertas de login

### 8. Conclusão
Reflexão sobre o aprendizado e boas práticas observadas durante o experimento.

# Metasploitable + Medusa Lab

**Resumo:** Laboratório para prática de ataques de força-bruta usando Medusa contra serviços vulneráveis (FTP, DVWA, SMB) em ambiente controlado (Kali + Metasploitable).

## Estrutura do repositório
- `lab-setup/` — instruções para recriar as VMs (VirtualBox host-only).
- `exploits-and-commands/` — passos e comandos usados em cada cenário.
- `wordlists/` — wordlists custom e notas sobre criação.
- `scans/` — saídas de nmap, smbclient, etc.
- `evidence/` — screenshots e capturas (sanitizadas).
- `docs/report.md` — relatório final com análise e recomendações.

## Requisitos
- VirtualBox
- 2 VMs: Kali Linux (atacante) e Metasploitable 2 (alvo)
- Rede: **Host-Only** 
- Ferramentas: `medusa`, `nmap`

## Como reproduzir (resumo)
1. Importar / criar VMs conforme `lab-setup/`.
2. Iniciar VMs na mesma rede host-only.
3. No Kali, executar `sudo ./scripts/run_medusa_ftp.sh` (ver `exploits-and-commands/ftp-medusa.md` para parâmetros).
4. Conferir resultados em `scans/` e evidências em `evidence/`.

## Segurança e ética
- Ambiente isolado e usado apenas para fins de aprendizado.
- **NÃO** executar ataques fora de ambiente autorizado.
- Dados sensíveis foram anonimizados — veja `evidence/README.md` para detalhes.

## Contribuições
Sugestões são bem-vindas. Abra uma issue ou PR.

## Licença
MIT © EriskSantos

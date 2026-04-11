## Objetivo
Usar OSINT, e se familiarizar com wayback machine ``https://archive.org/web/``

## Metodologia

Para despejar todos os links que são salvos no Wayback Machine, podemos usar a ferramenta chamada waybackurls. Hospedado em GitHub, 
nós podemos facilmente instalar isso em nossa máquina usando os comandos abaixo

## Comandos Utilizados
```bash
git clone https://github.com/tomnomnom/waybackurls

cd waybackurls

go build

ls -lah

./waybackurls tryhackme.com
```

## Resultado
mapeamento do site e dominios

```bash
https://tryhackme.com/.well-known/ai-plugin.json
https://tryhackme.com/.well-known/assetlinks.json
https://tryhackme.com/.well-known/dnt-policy.txt
https://tryhackme.com/.well-known/gpc.json
https://tryhackme.com/.well-known/nodeinfo
https://tryhackme.com/.well-known/openid-configuration
https://tryhackme.com/.well-known/security.txt
https://tryhackme.com/.well-known/trust.txt
```

## Lições Aprendidas

Wayback Machine é uma ferramenta de OSINT que arquiva versões históricas de sites.
A ferramenta waybackurls faz dump de todas as URLs indexadas para um domínio.

Google Dorks são consultas avançadas no Google para encontrar informações expostas publicamente, 

exemplos:
Para encontrar painéis administrativos: ``site:example.com inurl:admin``
Para desenterrar arquivos de log com senhas: ``filetype:log "password" site:example.com``
Para descobrir diretórios de backup: ``intitle:"index of" "backup" site:example.com``




## Conclusão 

A enumeração adequada é crucial para identificar potenciais vulnerabilidades em aplicações web. 
O uso das ferramentas e técnicas certas pode revelar informações valiosas que ajudam no planejamento de novos ataques.
E também otimizar ataques de força bruta envolve a criação de listas de palavras inteligentes, 
gerenciar parâmetros de ataque e evitar mecanismos de detecção, como limitação de taxa e bloqueio de conta.





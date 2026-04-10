## Objetivo
Usar o token previsível/fraco para bruteforçar com crunch e conseguir a nova senha redefinida 

## Vulnerabilidade Identificada
o token era um rang de 3 casas de numero aleatorio um token mto basico facilitando muito a brute. 
nesse exercicio eles mostraram como o codigo do token era:
```bash
$token = mt_rand(100, 200);
$query = $conn->prepare("UPDATE users SET reset_token = ? WHERE email = ?");
$query->bind_param("ss", $token, $email);
$query->execute();

```
mas isso poderia ser obtido atraves de recon

alem disso mostraram o endpoint do token ``http://enum.thm/labs/predictable_tokens/reset_password.php?token=123`` 

achar o endpoint é simples só interceptar com burp no esqueci senha ou redefinir senha 


## Metodologia

a metodologia foi simples utilizei o crunch pra gerar as varias senhas de 100 a 200. depois de gerar a lista otp.txt eu dei load nela com o burp. com a request do endpoint do token capturada marquei o token como posição de payload §123123§ rodei e esperei dps peguei oq saiu com leight diferente olhei o response e achei a nova senha. dps loguei com admin@admin.com e consegui a flag da room

## Comandos Utilizados

```bash
crunch 3 3 -o otp.txt -t %%% -s 100 -e 200
```

## Resultado
Flag =   ``THM{50_pr3d1ct4BL333!!}``

## Lições Aprendidas

se um token é simples previsível com apenas numeros. eu consigo explorar isso pra bruteforçar o token e achar o certo. assim conseguindo acesso a nova senha. mesmo sem acesso ao código, tokens com baixa entropia podem ser identificados observando o padrão nas respostas e testando ranges prováveis







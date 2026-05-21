## Objetivo
Fazer uma injeção de XSS na URL usando ``{{ }}`` para injetar como angular por conta do sandbox bypassável por design da linguagem, nesse lab foi necessario usar ``' ' .constructor.constructor`` para manipular o construtor do construtor da string que injetei vazia
para conseguir acessar o function() na linguagem e chamar o alert(1)

## Vulnerabilidade Identificada
é possivel usar ``{{}}`` para escapar do sandbox e mandar alert() manipulando o function() usando o constructor.constructor com uma string vazia 

## Metodologia
Fui na pagina na search e coloquei na url. ``{{' ' .constructor.constructor('alert(1)')()}}``

## Comandos Utilizados
<br>
<img width="1918" height="1008" alt="Screenshot From 2026-05-20 13-50-18" src="https://github.com/user-attachments/assets/2d450c94-c940-4e08-91ac-a3b82cfcb3c4" />

<br>

## Resultado
<br>
<img width="1920" height="922" alt="Screenshot From 2026-05-20 13-50-01" src="https://github.com/user-attachments/assets/09a04e82-cf73-487e-8d0a-4bec340bebcb" />
<br>

## Lições Aprendidas


Aprendi que AngularJS é legado e não se usam mais por ter um sandbox muito fraco. mas aprendi os principios basicos de escape sandbox chamando o construtor do construtor de uma string vazia
porque toda string tem um constructor e mexer no constructor do constructor da acesso ao function(). isso me ensinou que a cadeia de protótipos é acessível por propriedades e que a própria cadeia de protótipos do JavaScript
pode ser um vetor 

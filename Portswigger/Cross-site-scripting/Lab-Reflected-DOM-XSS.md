## Objetivo
Fazer uma injeção de XSS na URL usando escapando do contexto de string dentro do eval() e chamar alert()

## Vulnerabilidade Identificada
era possivel escapar do contexto de string usando ``\"`` a vulnerabilidade era DOM-based XSS o this.responseText era colocado direto em eval() sem sanitização
<br>
<img width="1910" height="973" alt="Screenshot From 2026-05-21 11-35-58" src="https://github.com/user-attachments/assets/876ea47a-6255-4175-b2a4-394e1857091b" />
<br>

## Metodologia
 o servidor escapa ``"`` mas não ``\``, então ``\"`` vira ``\\"`` no JSON, o que libera a aspa dupla no contexto do eval, permitindo quebrar a estrutura do objeto e injetar código. 
 então fui na pagina na search e coloquei na url. ``\"};alert(1)//``  o ``}`` fecha o objeto JSON que estava sendo construído pelo eval e o ``//`` comenta o ``"`` residual que sobra no final do JSON original, evitando erro de sintaxe

## Comandos Utilizados
<br>
<img width="1910" height="973" alt="Screenshot From 2026-05-21 12-25-41" src="https://github.com/user-attachments/assets/9adf7db5-4fdd-4a69-85e4-993e7d8ca0f7" />

<br>

## Resultado
<br>
<img width="1913" height="933" alt="Screenshot From 2026-05-21 12-27-14" src="https://github.com/user-attachments/assets/3b67fa66-e83d-4c02-91a4-16fb704841ad" />

<br>

## Lições Aprendidas

Aprendi a como navegar entre os recursos estáticos da aplicação pelo navegador e aprendi a identificar um sink perigoso (eval) lendo o código fonte

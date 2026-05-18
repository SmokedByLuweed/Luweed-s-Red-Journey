## Objetivo
Fazer uma injeção de XSS na URL do site e fazer pop up alert()

## Vulnerabilidade Identificada
a variável storeId é armazenada diretamente em document.write e o storeId vem de URLSearchParams.get('storeId') sem sanitização 
e como o browser é tolerante eu consegui injetar <script>alert()</script> diretamente na url usando a variável storeId

## Metodologia
cliquei em um produto do site e na URL escrevi ``productId=1&storeId=<script>alert(1)</script>``

## Comandos Utilizados
<br>
<img width="1920" height="1007" alt="Screenshot From 2026-05-17 23-01-21" src="https://github.com/user-attachments/assets/e0601efb-7586-48e5-88d1-ba73dd9b4767" />
<br>

## Resultado
<br>
<img width="1920" height="932" alt="Screenshot From 2026-05-17 23-05-24" src="https://github.com/user-attachments/assets/49658338-a16f-4540-a3fa-c8881fc15ef5" />
<br>

## Lições Aprendidas

aprendi que  o <script> se comportou diferente porque o parser HTML trata tags de script de forma especial mesmo dentro de contextos inesperados — 
e na solução oficial do lab q usa ``<img>`` aprendi que o ``<img>` dentro de <select> é ignorado pelo browser — elementos inválidos dentro de select são descartados. 
Então para o <img> funcionar precisa fechar o </select> primeiro se não o onerror nunca dispara

<br>
<img width="1920" height="1009" alt="Screenshot From 2026-05-17 23-10-51" src="https://github.com/user-attachments/assets/30879b41-bfd5-4d21-942e-54bff0838401" />
<br>
<br>
<img width="1920" height="1009" alt="Screenshot From 2026-05-17 23-13-07" src="https://github.com/user-attachments/assets/9758e1c5-0e4b-408e-aaf8-a989badcb77d" />
<br>
<br>
o <img onerror> é mais robusto porque o evento onerror dispara independentemente do estado do parser, 
enquanto <script> injetado via document.write pode não executar de forma confiável dependendo do browser.


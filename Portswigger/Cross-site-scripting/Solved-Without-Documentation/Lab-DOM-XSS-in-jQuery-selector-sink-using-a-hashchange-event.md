O evento hashchange só dispara de forma confiável depois que a página já carregou completamente e o listener foi registrado. 
Quando colocamos o payload direto na URL (ex: ``#');print(1);//``), o jQuery processa o hash antes do evento estar pronto, por isso não funciona.
a solução para o lab foi usar o exploit server com o seguinte iframe:
<br>
``<iframe src="https://0ab8002403238acb805935ce0080006b.web-security-academy.net/#" onload="this.src += '<img src=1 onerror=print()>'"></iframe>``
<img width="1920" height="935" alt="Screenshot From 2026-05-08 11-35-50" src="https://github.com/user-attachments/assets/9aff7bbb-34cf-43a6-bb43-a343131721cb" />
<br>
Explicação rápida:
O iframe carrega a página primeiro com hash vazio (#), pra página carregar normalmente e registrar o listener.
Quando o iframe termina de carregar, o onload executa.
this.src += '...'  modifica o hash dinamicamente depois do carregamento.
Essa mudança de hash dispara o evento hashchange no momento certo.
Aí o payload ``<img src=1 onerror=print()>`` é executado.
<br>
<br>
<img width="1920" height="935" alt="Screenshot From 2026-05-08 12-08-29" src="https://github.com/user-attachments/assets/03af683c-8826-4577-a77f-32dd7c4bf98d" />
<br>
<br>
<img width="1920" height="935" alt="Screenshot From 2026-05-08 12-08-52" src="https://github.com/user-attachments/assets/20a4c273-de74-487b-a564-99fb582d2a4f" />
<br>
<br>
<br>
Aprendi que em DOM XSS envolvendo eventos (principalmente hashchange), o timing é crítico. Nem sempre injetar direto funciona. 
Às vezes é necessário forçar o estado desejado depois do carregamento da página.

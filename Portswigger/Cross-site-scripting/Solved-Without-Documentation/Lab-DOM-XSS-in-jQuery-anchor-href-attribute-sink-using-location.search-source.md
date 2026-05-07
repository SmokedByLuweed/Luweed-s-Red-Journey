O lab explorava DOM XSS via jQuery. O código da página pegava o valor do parâmetro `returnPath` direto da URL e injetava sem sanitização no atributo `href` do link "Back". 
Como `href` aceita o protocolo `javascript:`, colocar `javascript:alert(1)` no `returnPath` fez o navegador executar o código ao clicar no link. O servidor nunca tocou no payload — 
tudo aconteceu no client-side, no DOM, em runtime.
<br>

<img width="1719" height="886" alt="image" src="https://github.com/user-attachments/assets/72ce0986-c9f5-4bc5-a5fc-7e9ceb450201" />

<br>
<br>
<img width="1920" height="1009" alt="Screenshot From 2026-05-07 20-04-39" src="https://github.com/user-attachments/assets/3af4ff0c-4425-4efe-848a-b2f0709811fa" />
<br>
<br>
<img width="1920" height="933" alt="Screenshot From 2026-05-07 20-07-02" src="https://github.com/user-attachments/assets/5405d4fb-3dd9-441d-81b9-85c532c48910" />

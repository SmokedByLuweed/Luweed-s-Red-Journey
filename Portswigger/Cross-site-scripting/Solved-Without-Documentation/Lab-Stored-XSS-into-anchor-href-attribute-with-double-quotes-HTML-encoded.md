quando o usuário envia um comentário com um website, o valor do website é colocado em href com uma sanitização incompleta encodando double quotes ``"`` mas como o href aceita protocolo 
``javascript:`` a sanitização se torna incompleta. a falha é a ausência de validação de scheme no servidor
<br>
<img width="1726" height="898" alt="Screenshot From 2026-05-15 10-23-31" src="https://github.com/user-attachments/assets/4fcd2d36-8c97-49b2-b5c3-767b602a66a9" />
<br>
assim, como o servidor não valida o scheme, o payload ``javascript:alert(1)`` é armazenado e executado quando o link é clicado
<br>
<img width="1910" height="928" alt="Screenshot From 2026-05-15 10-24-38" src="https://github.com/user-attachments/assets/7e1a6b52-999b-41fe-b38c-69d08e8c5667" />
<br>
<br>
<img width="1910" height="928" alt="Screenshot From 2026-05-15 10-26-11" src="https://github.com/user-attachments/assets/7edbee7c-631e-43f9-970b-bf166217f152" />
<br>
<img width="1910" height="928" alt="Screenshot From 2026-05-15 10-26-30" src="https://github.com/user-attachments/assets/4772c718-e506-4ff8-be0d-0d4ab2084fa8" />

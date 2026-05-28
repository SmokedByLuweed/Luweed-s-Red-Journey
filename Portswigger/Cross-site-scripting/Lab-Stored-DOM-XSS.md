## Objetivo
Fazer um XSS stored na sessão de comentários

## Vulnerabilidade Identificada
o servidor sanitiza parcialmente o input, mas o comentário é renderizado via innerHTML, que faz parsing do HTML e executa o payload que sobrou após a sanitização incompleta,
assim usando ``<>`` é possivel forçar a sanitização a gastar sua lógica no primeiro par de colchetes vazio e depois injetar ``<img src=x onerror ="alert(1)">``

## Metodologia
Fui em uma sesão de comentário em um post do blog, e injetei na sessão de comentários ``<><img src=x onerror ="alert(1)">`` depois voltei para a página do blog para executar 

## Comandos Utilizados
<br>
<img width="1920" height="1003" alt="image" src="https://github.com/user-attachments/assets/3a142949-99e5-4306-9477-e196da763b79" />
<br>

## Resultado

<br>
<img width="1920" height="1003" alt="image" src="https://github.com/user-attachments/assets/9908fb47-aaac-4152-a933-25098d1475c5" />
<br>
<br>
<img width="1920" height="933" alt="image" src="https://github.com/user-attachments/assets/5f18b6bd-01a4-47e2-a5b4-dc7f65dcb2f1" />
<br>
<br>

## Lições Aprendidas

Aprendi que sanitização incompleta combinada com um sink que faz parsing de HTML (como innerHTML) 
permite injeção de código mesmo quando há proteção parcial no servidor 

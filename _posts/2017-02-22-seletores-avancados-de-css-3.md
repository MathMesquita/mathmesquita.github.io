---
layout: post
title: "Pseudo-classes avançadas de CSS"
image: "/assets/seletores-avancados-de-css/cover3.jpg"
date:   2017-02-22 08:00:00 -0200
color_template: "css"
tags: css3 tutorial
resume: "Você sabia que consegue selecionar somente os elementos ímpares com CSS? Não? Descubra como !"
topic: css
comments: true
---

### Introdução

Fala pessoal, esse é mais um post da série **seletores avançados de CSS**, como explicado no post anterior, [Pseudo-classes de CSS](https://mathmesquita.me/2017/01/25/seletores-avancados-de-css-2.html), as pseudo-classes são todos os seletores iniciados com *:*, dois pontos.

Por existir várias *pseudo-classes* diferentes, não consegui abordar todas no primeiro post e vimos somente as mais simples, **algumas que poucos sabiam que existiam**, mas simples. Dessa forma no post de hoje trataremos as mais avançadas, as *pseudo-classes* que te possibilitam selecionar elementos em ordem matemática ou não selecionar baseado em seu seletor por exemplo.

Espero que gostem, e vamos por as mãos na massa !

### Quais pseudo-classes veremos?

Para não sair do padrão da série, vamos começar falando quais pseudo-classes iremos ver aqui, ajudando também a galera que vai revisitar essa página pra dar uma olhada naquele seletor que *lembrou mais ou menos*.

+ [:link ](#link)
+ [:visited](#visited)
+ [:root](#root)
+ [:target](#target)
+ [:not(seletor)](#not)
+ [:first-child](#fchild)
+ [:last-child](#lchild)
+ [:only-child](#ochild)
+ [:first-of-type](#ftype)
+ [:last-of-type](#ltype)
+ [:only-of-type](#otype)
+ [:nth-child(n)](#nchild)
+ [:nth-last-child(n)](#nlchild)
+ [:nth-of-type(n)](#ntype)
+ [:nth-last-of-type(n)](#nltype)

Os elementos *:link* e *:visited* não são muito avançados, eles estão nessa lista pois no post anterior a lista ja estava muito grande, <strike>não que essa não tenha ficado...</strike>

#### <a name="link"></a>:link
A pseudo-classe [:link](https://www.w3schools.com/cssref/sel_link.asp), seleciona todos os links que não foram visitados ainda.

#### <a name="visited"></a>:visited
A pseudo-classe [:visited](https://www.w3schools.com/cssref/sel_visited.asp), faz o oposto de *:link* e seleciona todos os links que ja foram visitados.

#### <a name="root"></a>:root
A pseudo-classe [:root](https://www.w3schools.com/cssref/sel_root.asp), irá selecionar o :root da sua página, que é o elemento *html*.

#### <a name="target"></a>:target
A pseudo-classe [:target](https://www.w3schools.com/cssref/sel_target.asp), selecionará o elemento **âncora da url**, que é quando voce clica em algum link com o href igual a *#alguma-coisa*, esse é o elemento ancora.

#### <a name="not"></a>:not(seletor)
A pseudo-classe [:not(seletor)](https://www.w3schools.com/cssref/sel_not.asp), seleciona todos os elementos que **não** possuem aquele seletor.

#### <a name="fchild"></a>:first-child
A pseudo-classe [:first-child](https://www.w3schools.com/cssref/sel_firstchild.asp), seleciona o elemento que é o **primeiro filho** de um elemento pai.

#### <a name="lchild"></a>:last-child
A pseudo-classe [:last-child](https://www.w3schools.com/cssref/sel_last-child.asp), como o nome ja diz, selecionará o **último filho** de um elemento pai.

#### <a name="ochild"></a>:only-child
A pseudo-classe [:only-child](https://www.w3schools.com/cssref/sel_only-child.asp), serve para selecionar aquele elemento que é **filho único** do elemento *pai*.

#### <a name="ftype"></a>:first-of-type
A pseudo-classe [:first-of-type](https://www.w3schools.com/cssref/sel_first-of-type.asp), selecionará o **primeiro elemento de determinado tipo**, *"tipo"* no caso será se o elemento é uma *div*, ou um *span* etc.

#### <a name="ltype"></a>:last-of-type
A pseudo-classe [:last-of-type](https://www.w3schools.com/cssref/sel_last-of-type.asp), tem comportamento inverso ao *:first-of-type* e seleciona o **último elemento de determinado tipo**.

#### <a name="otype"></a>:only-of-type
A pseudo-classe [:only-of-type](https://www.w3schools.com/cssref/sel_only-of-type.asp), tem comportamento idêntico ao *:only-child* selecionando apenas o **filho único** do elemento pai, porém será baseado no tipo do elemento, **se é uma 'div', um 'span' ou um 'p'** por exemplo, dessa forma no *:only-of-type* você pode ter vários elementos de *tipos* diferentes, e selecioná-los desde que sejam os **únicos daquele tipo naquele nível** da árvore DOM.

### Exemplos

Antes de continuarmos para a próxima parte que será um pouco mais complicada por ter fórmulas que deverão ser passadas como parâmetros para as pseudo-classes, vamos dar uma olhada nos exemplos de todas as pseudo-classes vistas até agora.
<p data-height="265" data-theme-id="0" data-slug-hash="OpyGyY" data-default-tab="result" data-user="mathmesquita" data-embed-version="2" data-pen-title="Pseudo-classes avançadas" class="codepen">See the Pen <a href="http://codepen.io/mathmesquita/pen/OpyGyY/">Pseudo-classes avançadas</a> by Matheus Mesquita (<a href="http://codepen.io/mathmesquita">@mathmesquita</a>) on <a href="http://codepen.io">CodePen</a>.</p>
<script async src="https://production-assets.codepen.io/assets/embed/ei.js"></script>

### Pseudo-classes com fórmulas
#### <a name="nchild"></a>:nth-child(n)
A pseudo-classe [:nth-child(n)](https://www.w3schools.com/cssref/sel_nth-child.asp), irá selecionar todos os elementos que se adequem a uma fórmula, na ordem do **primeiro para o último**.

#### <a name="nlchild"></a>:nth-last-child(n)
A pseudo-classe [:nth-last-child(n)](https://www.w3schools.com/cssref/sel_nth-last-child.asp), seleciona todos os elementos que se adequem a uma fórmula, na ordem do **último para o primeiro**.

#### <a name="ntype"></a>:nth-of-type(n)
A pseudo-classe [:nth-of-type(n)](https://www.w3schools.com/cssref/sel_nth-of-type.asp), também se comportará como o *:nth-child(n)* porém irá selecionar somente os de **determinado tipo** que sejam selecionados pela fórmula passada como parâmetro, na ordem do **primeiro para o último**.

#### <a name="nltype"></a>:nth-last-of-type(n)
A pseudo-classe [:nth-last-of-type(n)](https://www.w3schools.com/cssref/sel_nth-last-of-type.asp), tem o mesmo comportamento que *:nth-of-type* porém a ordem que a fórmula será aplicada é do **último para o primeiro**.

### As fórmulas aceitas

Antes de falarmos das **fórmulas**, *tenho que esclarecer uma coisa*, estarei usando sempre a **pseudo-classe** *:nth-child(n)* porém as fórmulas são válidas para qualquer outra que também aceite uma fórmula como parâmetro, sendo assim você que irá decidir qual pseudo-classe se adequa mais ao seu caso.

As fórmulas devem ser sempre descritas por **(an +- b)**, tal que, **a** será *"o tamanho do pulo em cada passada", podendo ser negativo ou positivo*, **n** irá ser cada iteração na função *começando em zero* e **b** será o *coeficiente inicial*.

**Alguns exemplos de uso:**

#### Selecionando elementros pares
Caso quisessemos selecionar todos os **elementos filhos** que sejam pares, iriamos passar a fórmula *:nth-child(2n)*, pois começando em zero, ela iria em cada iteração multiplicar n por dois resultando em um número par, _[2\*0=0, 2\*1=2, 2\*2=4, 2\*3=6, 8, 10, 12, 2\*n...]_.

#### Selecionando elementos impares
Para selecionarmos os **elementos filhos** impares, a fórmula seria *:nth-child(2n + 1)*, dessa forma o resultado das interações ficaria igual a _[2\*0+1=1, 2\*1+1=3, 2\*2+1=5, 7, 9, 11, 2\*n+1...]_.

#### Impares e Pares de forma fácil
Apesar de termos mostrado o uso das fórmulas com o exemplo de selecionar números *impares e pares*, não precisamos nos preocupar muito nesses casos, o CSS ja possui duas palavras-chave que tratam de selecionar numeros pares e impares, elas são **(even)** e **(odd)** respectivamente. 

#### Selecionando os 3 primeiros itens
Também podemos selecionar apenas os **3 primeiros** por exemplo, com a fórmula *:nth-child(-n + 3)*, nesse caso é importante lembrar que a fórmula tem que estar sempre no mesmo formato de *(an +- b)*, sendo que **a** pode ser um número negativo.

#### Selecionando os 3 últimos itens
E para selecionarmos os **3 últimos**, utilizaremos a mesma fórmula, porém com a *pseudo-classe* **:nth-last-child**, que basicamente faz a mesma coisa que child, porém na direção de baixo para cima, ou último para o primeiro. Nosso css ficaria *:nth-last-child(-n + 3)*.

#### Selecionando elementos específicos
Nada te impede de selecionar algum elemento com um número específico também, caso queira selecionar o *terceiro* elemento, você poderia utilizar *:nth-child(3)*, e para selecionar o *penúltimo* utilizar *:nth-last-child(2)*.

### Exemplos
<p data-height="265" data-theme-id="0" data-slug-hash="KWVMwr" data-default-tab="result" data-user="mathmesquita" data-embed-version="2" data-pen-title="Pseudo-classes avançadas" class="codepen">See the Pen <a href="http://codepen.io/mathmesquita/pen/KWVMwr/">Pseudo-classes avançadas</a> by Matheus Mesquita (<a href="http://codepen.io/mathmesquita">@mathmesquita</a>) on <a href="http://codepen.io">CodePen</a>.</p>

### Conclusão

É, deixei de por alguns seletores no post passado por achar que tinha ficado grande demais, talvez tenha me enganado quanto ao tamanho que esse aqui teria.

De qualquer forma, é isso ai, conseguimos cobrir todas as outras pseudo-classes que faltavam, acho que vou mudar o nome dessa série para **seletores CSS do básico ao avançado**, por que foi o que acabou acontecendo com os posts. Ainda falta mais um, nele iremos falar dos pseudo-elementos que são basicamente seletores que criam "elementos" no seu DOM, te permitindo fazer muitas coisas com eles.

Por fim, muito **Obrigado** para quem leu até aqui, e para quem leu só metade também, e para quem só entrou no link, o interesse em vir ler o post ja é algo gratificante para mim ! Espero que tenham gostado, creio que depois dessa série irei me afastar um pouco dos posts falando sobre CSS e focar um pouco mais na parte difícil do front-end que é o javascript.

Dúvidas, sugestões, dicas, um oi e qualquer outra interação, estarei nos comentários ! Obrigado novamente e até o próximo post galera !

---
layout: post
title: "Programação Funcional \"for Dummies\""
image: "/assets/fpjs/cover.jpg"
date:   2017-02-17 08:00:00 -0200
color_template: "javascript"
tags: javascript hard
resume: "Todos estão falando sobre, mas o que é isso? Venha descobrir de maneira for dummies algo que não é nada complicado"
topic: javascript
comments: true
---

### Introdução

Bom dia, boa tarde ou boa noite, o post que estou escrevendo hoje ja estava na lista de *"posts-to-do"* a um bom tempo.

**Functional Programming** sempre foi um daqueles tópicos que me deixava nervoso, porque achava ser algo super complicado e que eu nunca entenderia, mas quando tomei coragem, <strike>ou vergonha na cara</strike>, descobri que não é nenhum bicho de sete cabeças, mas tentam deixá-lo dificil *talvez para parecer algo mais robusto*, ja que ajuda em tantos pontos na sua aplicação.

Nesse post irei falar de uma maneira simples e direta, *sem frescuras*, o que é *FP*(Functional Programming) e quais suas vantagens frente a outros paradigmas de programação.

### Paradigmas de programação

Se esse termo é algo estranho para você, lembre das suas aulas de programação quando te contaram sobre **Orientação Objetos**, nesse momento estavam lhe ensinando um de vários paradigmas de programação.

E nem sempre é algo difícil, a vida inteira trabalhamos com um paradigma que pode ser considerado o mais simples de todos, **Imperative Programming**, ou *programação imperativa*, que é o tradicional código linha por linha, tipo aqueles algoritmos que fazemos quando estamos aprendendo lógica, literalmente mandando o computador fazer as coisas por nós **"pegue o numero do usuário, multiplique por 2, imprima esse numero na tela"**.

Esses dois são talvez os mais comuns, mas existem **N** outros tipos, e variações de tipos existentes, a *orientação a objetos* tem como parte do conceito a **hierarquia**, e esse conceito também está presente outros paradigmas como a **Prototypal Inheritance**.

Enfim, existem muitos paradigmas e talvez até possamos fazer posts falando de todos eles, caso queiram, não deixe de falar nos comentários !

### Paradigma da Programação Funcional

A [programação funcional](https://pt.wikipedia.org/wiki/Programa%C3%A7%C3%A3o_funcional, _blank), tem como principal pilar não realizar alterações no estado da sua aplicação, e isso se dá trabalhando com muitas funções que retornam novos valores sem alterar os passados.

Pense na [programação imperativa](https://pt.wikipedia.org/wiki/Programa%C3%A7%C3%A3o_imperativa, _blank), você declarava uma variavel *a* e ia alterando ela pela *linha do tempo* do código e isso fazia o **estado** inicial de *a* não ser mais o mesmo.

<div class="code javascript">
<pre><code>
var a = 4;

a = a + 2;

a = a * 2;

console.log(a); // 12

</code></pre>
</div>

No exemplo acima de *programação imperativa* conseguimos observar que a nossa variável *a* assumiu vários valores até sua impressão.

<div class="code javascript">
<pre><code>
var a = 4;

var b = soma(a, 2);

var c = multiplica(b, 2);

console.log(c); // 12

function soma(n, plus){
   return n + plus;
}

function multiplica(n, multiplier){
   return n * multiplier;
}

</code></pre>
</div>

O mesmo exemplo, só que agora escrito de forma *funcional*, se prestarem atenção vocês verão que o output final será o mesmo, o valor **12**, porém nesse segundo exemplo, em nenhum momento alteramos o valor da variável *a*.

Claro que esse exemplo está muito simples, e dessa forma deixamos algo fácil extremamente complicado. *Essa forma de pensar está totalmente correta* e é por isso que utilizamos programação imperativa em coisas bobas, mas agora pense em uma aplicação robusta, onde você tem várias variáveis que são utilizadas em diversos lugares da mesma, talvez alterar uma variável pode trazer **side effects não desejados** ocasionando grandes problemas no seu programa.

### Funções Puras

Uma coisa importante a se notar da **programação funcional**, é a utilização do que chamamos de *funções puras*, uma função para ser considerada pura nesse paradigma ela deve ser:

- Independencia, não acessa variáveis externas a ela
- Não ser constante, sempre ter uma(ou mais) entrada(s) e retornar algo baseado nelas
- Consistencia, retornar sempre o mesmo resultado dado a mesma entrada


<div class="code javascript">
<pre><code>
var a = 4;

function soma(plus){
   a += plus;
   // acesso a variável 'a' que está
   // fora da função soma
   console.log(a);
}

soma(2); // 6
soma(2); // 8
soma(2); // 10

</code></pre>
</div>

Aqui temos um exemplo do que **NÃO** seria uma função pura, ela representa totalmente o inverso do que uma *função pura* deve ser, **possui acesso a variável externa** *a*, **não retorna nenhum valor** e **dado a mesma entrada *2* mostra resultados diferentes**.

Para purificarmos essa função nós teriamos que fazer o seguinte.

<div class="code javascript">
<pre><code>
var a = 4;

function soma(n, plus){
   return n + plus;
}

console.log(soma(a, 2)); // 6
console.log(soma(a, 2)); // 6

console.log(soma(a, 3)); // 7
console.log(soma(a, 3)); // 7

</code></pre>
</div>

Como podem ver, agora a nossa função atende todos os nossos requisitos, **ela não acessa nenhuma variável externa**, **possui entradas que geram saidas** e **sempre retorna o mesmo resultado dado uma mesma entrada**.

### Testes

Tendo uma aplicação baseada inteiramente em funções puras, seus testes ficam bem mais fáceis de serem executados, afinal todas os retornos devem **SEMPRE** ser previsiveis.

<div class="code javascript">
<pre><code>
function soma(n, plus){
   return n + plus;
}

test("Soma 4 + 2 resultando em 6", () => {
   expect(soma(4, 2)).toBe(6);
});

</code></pre>
</div>

Você tendo funções puras, seus testes dificilmente irão quebrar, pois suas funções não são dependentes de variáveis espalhadas pelo sistema ou que podem ser alteradas por outras funções, **o retorno da sua função será dependente somente da sua entrada**. 

Na nossa função *soma(n, plus)*, sempre que **4** e **2** forem passados como parametros da função, ela irá retornar **6**, e nunca outro número.

### Conclusão

Apesar de eu ter usado javascript o tempo inteiro nesse post e com exemplos bobinhos, isso tudo foi pensando somente em como passar didaticamente o que é o **paradigma de programação funcional**. 

No javascript temos várias funções que nos auxiliam a manter esse paradigma, e é o que veremos no próximo post falando sobre *programação funcional no javascript*, com exemplos de tarefas reais que você pode realizar.

Espero que todos tenham conseguido entender, e gostado do post, para aqueles que não conseguiram entender não fiquem acanhados e mandem suas dúvidas nos comentários.

Obrigado pela atenção e qualquer sugestão ou crítica os comentários estão sempre abertos ! Até o próximo post e prometo que não demorarei para fazer o **programação funcional no javascript**. Abraços !

---
layout: post
title: "Function Composition no Javascript"
image: "/assets/fpjs/cover2.jpg"
date:   2017-03-12 08:00:00 -0200
color_template: "javascript"
tags: FP javascript avançado
resume: "Comece a compor filas de funções para dar um próximo passo na utilização de programação funcional."
topic: js
comments: true
---

### Introdução

Bom dia, boa tarde ou boa noite ! Voltando à [programação funcional](https://mathmesquita.me/2017/02/17/programacao-funcional-for-dummies.html), hoje veremos as *"funções compostas"* e como deixar seu código mais legível com elas ao programar de maneira funcional.

No início pode ser difícil de pensar em como  que *"compor funções"* irá deixar seu código legível, pretendo, com exemplos, mostrar que após uma certa quantidade de funções, *compô-las* será a maneira mais fácil de você não parar no canto escuro da sala com um dedo na boca e cheio de medo do seu código.

Dito isso tudo, bora começar o post.

### O que são as funções compostas?

Esse é o momento que eu peço para você retornar ao seu ensino médio, no primeiro ano, quando você aprendeu sobre **funções compostas**, se você não lembra o que são, não tenha medo, é simplesmente o ato de você encadear funções uma dentro da outra. 

Para ilustrar, nós temos a seguinte função composta *a(b(c(x)))*, e o que ta acontecendo nessa função é o seguinte, nós temos 3 funções *a(x)*, *b(x)* e *c(x)* e as estamos passando o resultado de uma como parametro na outra. A primeira função a ser executada é a *c(x)* o resultado dela vira o **x** de *b(x)*, quando executada *b(x)* o resultado vira o **x** de *a(x)* e no final temos o resultado da função composta.

### Voltando para programação

Constantemente ao programarmos de maneira funcional nós vemos funções encadeadas, afinal se você está usando *funções puras* e simples, você provavelmente está tendo que passar alguma *variável* através de uma fila de funções. 

Demonstrando em código.

<div class="code javascript">
<pre><code>
// começamos com nossa variável imutável valendo 3
let a = 3;
// multiplicamos por 5 e atribuimos o resultado em b
let b = multiplicaPorCinco(a);
// dividimos por 3 e atribuimos a divisão em c
let c = dividePorTres(b);
// somamos sete e atribuimos em d
let d = somaSete(c);
// diminuimos cinco e atribuimos a ultima variável da nossa fila
let e = diminuiCinco(d);

console.log(e); // 7

</code></pre>
</div>

O problema até é resolvido se programarmos dessa maneira, mas tem um problema, com isso sujamos todo o nosso escopo com todas essas variáveis auxiliares que se tornam inúteis na próxima linha.

Um maneira de resolver o problema das variáveis auxiliares, é compor as funções exatamente com fazemos na matemática.

<div class="code javascript">
<pre><code>

let a = 3;

// \/ ESSE É O ULTIMO PASSO
// diminui cinco da soma e atribui o resultado na variavel e
let e = diminuiCinco(
   // sete foi somado a divisão e novamente passada como parâmetro
   somaSete(
      // dividido por tres e passado novamente como parâmetro
      dividePorTres(
         // multiplicado por cinco e passado como parâmetro
         multiplicaPorCinco(
            // a passado como parâmetro
            a
            // /\ COMEÇA AQUI
         )
      )
   )
);

console.log(e); // 7

</code></pre>
</div>

O resultado final será o mesmo e não estaremos sujando nosso escopo, **problema resolvido** !

### Legibilidade é importante

É, talvez não tão resolvido assim, apesar de continuar funcionando nosso código, agora conseguimos deixá-lo totalmente incompreensível. 

*"MAS EU CONSEGUI ENTENDER"*, meus parabéns, também consegui entender, mas pense no caso de você adicionar mais funções nessa fila de execução, daqui a pouco você terá que utilizar o scroll horizontal do seu editor para ver qual será a próxima função !

Para resolvermos esse problema existem funções que conseguimos montar nossa fila e atribui-la em uma variável. Algumas libraries como o **redux** ja possuem implementações dessa função então não é necessário você monta-lá manualmente, mas caso você não esteja usando *redux* ou qualquer outra lib que tenha a função **compose()** implementada, deixe que eu te ensino a fazer essa função super complicada.

### Compondo funções com Compose

<div class="code javascript">
<pre><code>
// versão muitas linhas(podia ser pior)
function compose(...fns){
   return function(x){
      return fns.reduceRight(
         function(val, fn){
            return fn(val);
         }, x );
   }
}

// versão uma linha
let compose = (...fns) => x => fns.reduceRight((v, f) => f(v), x);

</code></pre>
</div>

Se você não conseguiu entender muito bem o que aconteceu ali em cima, deixe que eu explico.

Primeiro temos a declaração da nossa função **compose**, ela que será a função que aceitará como parâmetro todas as funções que queremos compor, no final ela retornará uma função que será a nossa *função composta*.

A função retornada pelo **compose** aceita um parâmetro **x**, que será a nossa primeira entrada da fila, nos exemplos anteriores nós passariamos a variável **a** como parâmetro para a função que compomos.

Dentro da função retornada nós executamos um loop no array das funções que passamos como parâmetro para a função **compose** com o método [.reduceRight(fn, valorInicial)](https://developer.mozilla.org/pt-BR/docs/Web/JavaScript/Reference/Global_Objects/Array/ReduceRight), esse métododo do prototype dos *Arrays* irá executar uma função, que passaremos como primeiro parâmetro, em cada elemento do array **de forma decrescente**, do último para o primeiro.

A função que passamos como parâmetro para o **.reduceRight()** irá receber a cada loop dois valores como parâmetro, o primeiro parâmetro será **o valor retornado pela função anterior**, se for a primeira iteração esse valor será o valor inicial passado como segundo parâmetro do *.reduceRight()*. O **segundo parâmetro** irá receber o item do array da iteração atual, que no nosso caso será uma das funções que passamos como parâmetro para **compose**. E finalmente o *retorno* dessa função que passamos no primeiro parâmetro de *.reduceRight()* será o **valor resultante da função anterior aplicada na função atual** da nossa iteração.

Vamos ao exemplo.

<div class="code javascript">
<pre><code>
let compose = (...fns) => x => fns.reduceRight((v, f) => f(v), x);

// função compose irá retornar uma função que chamará
//    todas as funções que passamos por parâmetro
//    da ultima para a primeira, sendo assim
//    multiplicaPorCinco será chamada primeiro, 
//    depois dividePorTres e subindo
let operacoesMatematicas = compose(
   diminuiCinco,
   somaSete,
   dividePorTres,
   multiplicaPorCinco
);

let a = 3;

// passamos nossa variavel a como parâmetro para a função
//    composta operacoesMatematicas, e nossa função tratará
//    de realizar todas as funções na nossa variavel
//    e retornar o resultado final, que é 7
let resA = operacoesMatematicas(a);
console.log(resA); // 7

// outra vantagem é a reutilização da função composta
//    assim você não precisa escrever todo aquele processo
//    diversas vezes, ou escrever uma função separada
//    que irá cuidar disso pra você
let b = 4;
let resB = operacoesMatemateicas(b);
let c = 5;
let resC = operacoesMatemateicas(c);
let d = 6;
let resD = operacoesMatemateicas(d);

</code></pre>
</div>

Agora está bem mais legível do que nossas outras duas soluções não acham ? Se te incomoda o fato de estar invertido a ordem das funções que passamos como parâmetro, não se preocupe, não existe uma regra sobre como funções que compõem outras devem ser realizadas. Mas comumente utilizamos o nome **compose** para as funções que irão compor na ordem de último para primeiro, e **pipe** para as funções que compoem do primeiro para o último.

Novamente, algumas libraries ja possuem implementação dessas funções, então nem sempre será necessário você implementá-las manualmente, mas por via das dúvidas, fica abaixo como seria a implementação de uma função de composição no estilo **pipe**.

<div class="code javascript">
<pre><code>
// alteramos somente o .reduceRight para .reduce, que irá
//    realizar um loop na ordem crescente do array,
//    da primeira função para a última
let pipe = (...fns) => x => fns.reduce((v, f) => f(v), x);

</code></pre>
</div>

Apesar de ficar talvez *"mais legível"*, se você notar atentamente verá que a forma da última para primeira reflete a realidade de uma função composta na matemática **a(b(c(x)))**, onde a última função que seria *c(x)* é na verdade a primeira a ser realizada, e consequentemente as outras com o resultado obtido nela.

Enfim, isso cabe a você e seu time decidirem o que usar, mas que fique avisado que GRANDE parte utiliza **compose** invés de **pipe**, e eu to nesse meio.

### Conclusão

Por hoje é só, o post para por aqui e espero que você não fique aguardando para achar onde utilizar essa ferramenta muito útil quando desenvolvemos de forma funcional. Por via das dúvidas, eu ja dei um exemplo no meu post [higher-order components no react](https://mathmesquita.me/2017/03/01/higher-order-components-no-react.html) de como utilizar uma função compose para encadear vários *hoc's* no seu componente.

Atualmente tem muitas pessoas realizando *pipes* de funções com **Promise.resolve().then()**, depois farei um post falando sobre essa forma de realizar encadeamento de funções, muito boa mas com usos particulares, *compose* continua sendo uma boa alternativa na maioria dos casos.

Enfim, muito obrigado pela sua atenção, espero que tenha gostado do post, e caso não tenha entendido algo, ou acha que fiquei devendo algum tópico, me avise nos comentários ! Se você achou muito fácil não se preocupe, pois realmente é algo bem simples, e que poucas pessoas sabem utilizar, apesar de suas várias vantagens.

Abraços e até o próximo post ! Obrigado de novo ! :D


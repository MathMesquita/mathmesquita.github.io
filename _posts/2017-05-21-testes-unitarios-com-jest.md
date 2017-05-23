---
layout: post
title: "Testes unitários com JEST !"
image: "/assets/jest/cover.jpg"
date:   2017-05-21 08:00:00 -0200
color_template: "javascript"
tags: tests javascript intermediário jest
resume: O post pra quem sabe que precisa testar seu código, mas não tem idéia por onde começar !
topic: javascript
comments: true
---

### Introdução

Olar pessoal, bom dia, tarde ou noite ! Tem mais de um mês que eu fiz meu último post, muitas coisas aconteceram !! E em breve eu poderei explicar o que foi exatamente, até lá ficará em segredo. _Shhh_

Hoje vamos aprender um pouco sobre testes, principalmente como configurar seu projeto para fazê-los e como executá-los, os testes que iremos abordar serão os [unitários](https://pt.wikipedia.org/wiki/Teste_de_unidade)*(ou de unidade)*, e eles são só mais uma categoria de testes entre outros existentes como [A/B](https://pt.wikipedia.org/wiki/Teste_A/B), de [integração](https://pt.wikipedia.org/wiki/Teste_de_integração), de [aceitação](https://pt.wikipedia.org/wiki/Teste_de_aceitação) e [etc](https://pt.wikipedia.org/wiki/Teste_de_software).

Também vou falar um pouco sobre a importância de testar seu código e coisas que poderiam ser evitadas caso você começasse a testar, ja deixando aqui uma pista que no front-end vários *alt+tab* ou *alt+tab, F5* seriam evitados, pelo simples fato de você ter uma fonte de confiança que serão os verdinhos significando que as funções estão retornando o que era esperado. 

Tudo isso acompanhado de um exemplo prático de TDD.

Sem mais delongas, partiu pro post que eu estava morto de vontade de escrever !

### Testes Unitários

Os **testes unitários** serão os responsáveis por testar as *unidades* de nosso software, e elas podem se resumir basicamente a nossas funções, dessa forma podemos reescrever que os testes unitários serão os testes das nossas funções, analisando como elas se comportam e se dado um determinado *input* ela está retornando outro *output* esperado.

Aqui entra a parte que eu lembro vocês de um **outro post meu**, o [sobre programação funcional](https://mathmesquita.me/2017/02/17/programacao-funcional-for-dummies.html), se você o leu provavelmente lembra que eu avisei que ao programarmos funcionalmente nosssos testes ficariam bem mais simples, e agora eu mostrarei o por que.

Se nossas funções forem puras, não precisaremos montar muitos **mocks** que são ambientes "simulados" para nossos testes, como por exemplo uma função que depende de acesso a uma variável externa *a* e precisariamos simular uma variavel *a* em um escopo superior para a função funcionar corretamente e conseguirmos testá-la.

#### para que testar?

Se você não testa seu código você provavelmente ja passou por aquele momento que você precisa fazer uma reordanação muito louca baseada em várias coisas no seu array e precisou atualizar a página do seu app várias vezes para ver se o resultado estava correto no console, eu creio que você achou isso muito chato, e realmente foi !

Todo esse trabalho de: fazer alteração no algoritmo > mudar para tela do navegador > conferir o resultado > fazer alteração no algoritmo > ..... , poderia ter sido evitado com os testes unitários, em alguns editores como vscode você tem até um console dentro do próprio editor que da para ficar acompanhando se um teste é aprovado retornando o que realmente deveria estar retornando.

Pensando agora talvez seja difícil para você se convencer que testes facilitariam sua vida, mas confia em mim, dê uma chance a esse post e aos testes ! No inicio pode parecer que você está atrasando seu desenvolvimento, mas seu *"futuro eu"* irá agradecer na hora que ele fizer alguma alteração sem querer em uma função que ja estava funcionando e o teste dela simplesmente parar de funcionar pois ele quebrou a função.

Mas então, bora testar?

### Configurando o JEST

O framework de testes de iremos utilizar será o [Jest](http://facebook.github.io/jest/pt-BR/), e para integrarmos ele ao nosso projeto é muito fácil. Só precisamos rodar o comando **npm install jest --save-dev** que irá salvar o framework como uma dependencia de desenvolvimento.

Caso você não tenha um projeto criado, dentro de uma pasta vazia rode o comando *npm init* responda as questões que aparecerem na tela e seu projeto estará pronto para receber o jest.

Com o jest adicionado em nosso projeto, só precisamos criar o comando que irá realizar os testes para nós no arquivo *package.json*.

<div class="code javascript">
  <span class="file-name">package.json</span>
<pre><code>
{
  "name": "nome do seu projeto",
  "version": "1.0.0",
  "main": "index.js",
  "license": "MIT",
  "devDependencies": {
    "jest": "^20.0.3"
  },
  "scripts": {
    "test": "jest --watch"
  }
}

</code></pre>
</div>

Adicionando o script "test" toda vez que rodarmos *npm run test* o comando *jest* será acionado e nossos testes executados, a flag *--watch* serve para o jest ficar assistindo alterações nos arquivos, e reexecutar os testes automaticamente, assim não precisamos sempre ir no console e rodar *npm run test* manualmente todas as vezes.

Caso você queira executar o comando *jest* manualmente é necessário instalar o framework globalmente com o comando *npm install jest -g*, mas não indico fazer isso a menos que seja para propósitos de estudo, manter o comando encapsulado por um outro *test* é bom por que mais pra frente com algumas flags do framework pode se tornar muito chato escrever isso tudo toda vez que voce for testar seus arquivos. Além claro de se for o caso de ter mais de um framework de testes funcionando no mesmo projeto, você poderá executar os dois simultaneamente de forma mais fácil.

Por padrão o jest considera todos os files com a extensão *\*.test.js* como arquivos de teste, mas também poderiamos passar uma regex com um pattern no comando *jest*, ficando *jest regexPattern*.

### Anatomia de um teste unitário no jest

<div class="code javascript">
<pre><code>
// essa é a forma que um teste é escrito no framework jest
//  porém todos os frameworks possuem forma semelhante

// o jest nos diponibiliza uma função global `test` 
//  onde passamos uma string que será a responsável por 
//  descrever o que a unidade que estamos testando 
//  irá realizar, no nosso caso a função que estamos
//  testando deverá remover todos os espaços de uma string
test("todos os espaços são removidos da string", () => {
  // para manter nossos testes organizados é bom
  //  isolar os inputs, outputs e o valor que
  //  esperamos que o output receba após executado a função
  const input = "testes são bons"; // como vai entrar na função
  const expected = "testessãobons"; // como deve sair da função

  const output = removeSpaces(input); // resultado da função

  // aqui é onde acontece a magia, verificamos se o
  //  output realmente é igual ao valor esperado 
  //  com o matcher `.toBe` que é basicamente um `===`
  expect(output).toBe(expected);
});

// caso o output seja igual ao valor esperado 
//  nosso teste irá passar e poderemos ver no console
//  o verdinho sinalizando que nosso teste passou
//  caso contrário ficará vermelho e terá uma explicação
//  com o output que foi encontrado diferente do esperado

</code></pre>
</div>

### Testando se os testes estão funcionando

Sempre ao instalar algum framework de testes é bom testarmos se ele esta funcionando, sendo assim bora criar nosso arquivo de teste *jest.test.js* na raiz do projeto mesmo e dentro dele escrever o seguinte.

<div class="code javascript">
  <span class="file-name">jest.test.js</span>
<pre><code>
test("jest is working", () => {
  expect(true).toBe(false);
});

</code></pre>
</div>

Ao criarmos esse arquivo e executarmos *npm run test* nosso teste deverá falhar, afinal sabemos que *true* é diferente de *false*. Para vê-lo funcionar e confirmar que nosso framework está imprimindo os erros e os acertos é só realizar a alteração abaixo.

<div class="code javascript">
  <span class="file-name">jest.test.js</span>
<pre><code>
test("if jest is working", () => {
  expect(true).toBe(true);
});

</code></pre>
</div>

Sim, eu sei que fazer isso pode parecer ridículo, mas pensa só que graças a isso você pode ter *99.9% de certeza* que o problema não está no framework ou no ambiente e sim nas suas funções.

### Caso Real

Tudo pronto, explicado e testado, vamos a um exemplo real.

Supomos que chegou uma demanda no nosso trabalho de uma funcionalidade onde precisaremos criar uma função que irá somar todos os produtos do carrinho de um usuário. Primeiro criamos nosso teste:

<div class="code javascript">
  <span class="file-name">jest.test.js</span>
<pre><code>
const somaItemsCarrinho = require('./caminho/para/somaItemsCarrinho');

test("soma todos os produtos de um carrinho", () => {
  const input = [
    {
      nome: "produto 1",
      preco: 2.25,
      quant: 1,
    },
    {
      nome: "produto 2",
      preco: 3.5,
      quant: 3,
    },
    {
      nome: "produto 3",
      preco: 9.75,
      quant: 2,
    },
  ];

  const output = somaItemsCarrinho(input);

  const expected = 32.25;

  expect(output).toBe(expected);
});

// o ideal é você não parar nesse teste, 
//  fazer casos possível como inputs 
//  com tipos errados, e testar se o
//  tratamento de erros ta funcionando
//  da forma correta

</code></pre>
</div>

Rodamos nosso teste e ele falhará, agora vamos corrigir as falhas, a primeira delas é a inexistência do arquivo *somaItemsCarrinho.js*, vamos criá-lo com a função que irá retornar a soma dos items do carrinho, dado o input do exemplo.

<div class="code javascript">
  <span class="file-name">somaItemsCarrinho.js</span>
<pre><code>
module.exports = somaItemsCarrinho;

function somaItemsCarrinho(produtos){
  return produtos.reduce(function(total, produto){
    return total + produto.preco*produto.quant;
  }, 0);
};

</code></pre>
</div>

Criamos nosso arquivo e fazemos o código acima, ao salvarmos automaticamente o teste é executado e tudo estará ok, nosso teste está passando !

### ES6

Nós podemos utilizar ES6 nos nossos testes adicionando o transpiler [babel](https://babeljs.io/), também de forma bem fácil só precisamos instalar o preset do *es2015* com o comando **npm install --save-dev babel-preset-es2015** e criarmos o file *.babelrc* na raiz do nosso projeto. Lembrando que caso o seu projeto ja existisse e o babel ja estivesse configurado isso não seria necessário, e você poderia usar ES6 diretamente.

<div class="code javascript">
  <span class="file-name">.babelrc</span>
<pre><code>
{
  "presets":["es2015"]
}

</code></pre>
</div> 

Arquivo criado e dependência instalada, só vamos refatorar então nossos files

<div class="code javascript">
  <span class="file-name">jest.test.js</span>
<pre><code>
// usando import do ES6
import somaItemsCarrinho from './caminho/para/somaItemsCarrinho';

test("soma todos os produtos de um carrinho", () => {
  const input = [
    {
      nome: "produto 1",
      preco: 2.25,
      quant: 1,
    },
    {
      nome: "produto 2",
      preco: 3.5,
      quant: 3,
    },
    {
      nome: "produto 3",
      preco: 9.75,
      quant: 2,
    },
  ];

  const output = somaItemsCarrinho(input);

  const expected = 32.25;

  expect(output).toBe(expected);
});

</code></pre>
</div>

<div class="code javascript">
  <span class="file-name">somaItemsCarrinho.js</span>
<pre><code>
// essa função está abstraída para deixarmos 
//  nosso export mais simples, como ela não será
//  utilizada em nenhum outro arquivo, ela não
//  precisa ser testada, pois ela é "testada"
//  automaticamente quando testamos somaItemsCarrinho
const addProdutoAtualNoTotal = (total, produto) => total + produto.preco*produto.quant;

const somaItemsCarrinho = produtos => produtos.reduce(addProdutoAtualNoTotal, 0);

export default somaItemsCarrinho;

</code></pre>
</div>

### TDD - Desenvolvimento Orientado a Testes

Tudo refatorado, testes passando \*thumbs up\* ! Acabamos de fazer o [TDD](https://pt.wikipedia.org/wiki/Test_Driven_Development)*(Test Driven Development)*, desenvolvimento orientado a testes, que segue o seguinte ciclo.

![ciclo do desenvolvimento orientado a testes, escreva um teste que falhe, faça ele passar e refatore sua função da melhor forma](/assets/jest/tdd-circle-of-life.png)

Traduzindo:
- Escrever um teste que irá falhar
- Corrigir as falhas e fazer o teste ser aprovado
- Refatorar a unidade da melhor forma possível
- Ir para o próximo teste/repetir os passos anteriores

### React

O **Jest** por ser um framework do facebook têm uma integração muito boa com o **React** inclusive ja vem instalado com o **create-react-app**, nesse caso ao criar um app via o comando você só tem que botar os arquivos de teste dentro de um diretório com o nome *__tests__* que eles serão executados automaticamente.

[Nesse repositório](https://github.com/MathMesquita/swgame) vocês poderam encontrar um exemplo real de um projeto **React** com testes ! :D

### Conclusão

O post introdutório fica por aqui, o **Jest** nos proporciona mais uma [penca de coisas](http://facebook.github.io/jest/docs/pt-BR/using-matchers.html#content) e olhar sua documentação é essencial, pois em um post seria impossível mostrar a quantidade de coisas possíveis de se fazer. Quem não sabe inglês não se preocupe pois recentemente a documentação foi toda traduzida para o português brasileiro ! Caso você saiba inglês e queira ajudar a traduzir enquanto aprende é só [clicar aqui](https://crowdin.com/project/jest) e ajudar a revisar.

Espero ter conseguido mostrar o que é e para que servem os testes unitários, deixo aqui a minha **dica de amigo**, por mais que pareça difícil e chato realizar esse processo, com o tempo se torna algo bem natural e irá te fazer pensar de outra forma, exercitando sua capacidade de abstrair problemas uma característica muito boa na nossa área.

Obrigado a quem leu até aqui, caso tenham gostado do post falem nos comentários, também estou aberto a qualquer critica, sugestão ou dúvida ! Meu email também pode ser utilizado para tirar dúvidas caso você tenha receio/vergonha de expô-las para todo mundo nos comentários. *ps: e meu email pode ser achado na área de* [contato](/contato).

É isso aí ! Até o próximo post e valeu ! :D


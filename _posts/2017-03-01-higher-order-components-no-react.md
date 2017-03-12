---
layout: post
title: "Higher-Order Components no React"
image: "/assets/hoc-react/cover.jpg"
date:   2017-03-01 08:00:00 -0200
color_template: "react"
tags: React javascript avançado
resume: "Como utilizar HOCs para não ser repetitivo ao escrever componentes."
topic: react
comments: true
---

### Introdução

Olá pessoas, bom dia/tarde/noite, a partir de hoje vou focar um pouco mais na minha área preferida do front, que é o javascript e suas libs/frameworks e ferramentas diversas.

Vou começar hoje com um post que já irá explodir algumas mentes, ou trazer novas perspectivas aos problemas que enfrentamos no nosso dia-a-dia ao desenvolver WEBApps com ReactJS, e como solucioná-los de maneira mais inteligente e que não nos faça quebrar a regra que deveria estar gravada na cabeça de todo programador, DRY(*don't repeat yourself*).

Bora aprender logo sobre os HOC's por que os problemas não serão resolvidos sozinhos ! 

### O que são Higher-Order Components ?

Começando pela **explicação complicada**, um [HOC](https://facebook.github.io/react/docs/higher-order-components.html)*(sigla de Higher-Order Component)* é uma forma de utilizarmos conhecimentos de **factories** para reutilizar lógica comum a mais de um componente diferente.

A **explicação fácil** de *HOC's*, é simplesmente uma função que irá retornar um componente dado outro componente por parâmetro.

Claro que isso é somente a essência do *HOC*, ao retornar outro componente, nós conseguimos adicionar lógicas novas a eles, props e qualquer método que julguemos necessários.

Para melhor visualizar quando os **HOC's** serão uteis, vamos ao exemplo de um popup.

### Popups sem HOC

Não muito raro, temos popups em nossa aplicação responsáveis por várias coisas, em nosso exemplo os dois popups compartilham uma lógica semelhante, nesse caso, que é a ação de fechar o mesmo. Essa ação pode ser qualquer coisa, por exemplo uma action que irá ser despachada para sua store avisando sua aplicação que o popup que estava ativo foi fechado, ou algo do gênero.

<div class="code javascript">
    <span class="file-name">SignInPopup.js</span>
<pre><code>
import React, { Component } from 'react';

class SignInPopup extends Component {
   constructor(props){
      super(props);
   }

   closePopupHandler = () => {
      // executa a ação que irá fechar o popup
   }

   render(){
      return (
         &lt;div class="popup"&gt;
            &lt;button onClick={this.closePopupHandler}&gt;Fechar Popup&lt;/button&gt;
            // Resto do JSX
         &lt;/div&gt;
      )
   }
}

export default LoginPopup;

</code></pre>
</div>

O SignInPopup é um componente normal como qualquer outro, e agora o nosso SignUpPopup tem um método que a lógica é identica ao do SignInPopup.

<div class="code javascript">
    <span class="file-name">SignUpPopup.js</span>
<pre><code>
import React, { Component } from 'react';

class SignUpPopup extends Component {
   constructor(props){
      super(props);
   }

   closePopupHandler = () => {
      // executa a ação com mesma lógica que o componente SignInPopup
   }

   render(){
      return (
         &lt;div class="popup"&gt;
            &lt;button onClick={this.closePopupHandler}&gt;Fechar Popup&lt;/button&gt;
            // Resto do JSX
         &lt;/div&gt;
      )
   }
}

export default SignUpPopup;

</code></pre>
</div>

Os dois componentes estão ok, eles irão funcionar muito bem para seus propósitos, mas e se surgir a necessidade de criar outros popups? A melhor solução é criar vários componentes e copiar esse trecho de código? Creio que você respondeu não.

E é aí onde entra um caso que podemos solucionar utilizando *HOC's*, criando uma função que trate de adicionar essa lógica em todos os popups que criemos, não precisaremos nos repetir e poderemos refatorar todos os popups de forma a torná-los **stateless components**.

### Popups com HOC

Vamos começar criando nosso **HOC**, ele receberá o componente *Popup* como parâmetro, e retornará um novo componente com a propriedade *closePopup* injetada nesse componente.

<div class="code javascript">
    <span class="file-name">createPopup.js</span>
<pre><code>
import React from 'react';

const createPopup = Popup => class extends React.Component{
   constructor(props){
      super(props);
   }

   closePopupHandler = () => {
      // lógica para fechar o popup
   }

   render(){
      return <Popup closePopup={this.closePopupHandler} {...this.props} />;
   }
}

export default createPopup;

</code></pre>
</div>

Após nosso **HOC** estar criado, agora vamos refatorar nossos dois componentes de popup que estavamos usando anteriormente.

<div class="code javascript">
    <span class="file-name">SignInPopup.js</span>
<pre><code>
import React from 'react';

import createPopup from 'createPopup.js';

const SignInPopup = ({ closePopup, ...props }) => {
   return (
      &lt;div class="popup"&gt;
         &lt;button onClick={closePopup}&gt;Fechar Popup&lt;/button&gt;
         // Resto do JSX
      &lt;/div&gt;
   )
}

export default createPopup(SignInPopup);

</code></pre>
</div>
<div class="code javascript">
    <span class="file-name">SignUpPopup.js</span>
<pre><code>
import React from 'react';

import createPopup from 'createPopup.js';

const SignUpPopup = ({ closePopup, ...props }) => {
   return (
      &lt;div class="popup"&gt;
         &lt;button onClick={closePopup}&gt;Fechar Popup&lt;/button&gt;
         // Resto do JSX
      &lt;/div&gt;
   )
}

export default createPopup(SignUpPopup);

</code></pre>
</div>

Esses são nossos componentes refatorados, apesar de continuarmos repetindo o código no botão que é responsável por fechar o popup, esse problema podemos resolver de outra forma que não utilizando os **HOC's**, portanto não será abordado nesse post.

### Convenções

Existem algumas conveções indicadas pela própria documentação do React, elas são utilizadas pela comunidade e muitas vezes você se depará com elas em códigos que estão em produção, ou disponíveis pelo github.

#### Display Name

Quando inspecionamos os componentes pelo [React devTools](https://chrome.google.com/webstore/detail/react-developer-tools/fmkadmapgofadopljbjfkapdkoienihi), o nome que vemos nos componentes são definidos pela propriedade *displayName* dos mesmos, dessa forma para mantermos fácil de se ler e "honesto" nosso debugger. 

A convenção adotada, diz que devemos demonstrar que nossos componentes são hocs alterando o *displayName* para ser o nome do nosso HOC e entre parenteses o nome do componente que é o parâmetro do mesmo, *HocFunction(ReactComponent)*. 

Veja na imagem abaixo um exemplo do **HOC** *connect* do react-redux.

![Imagem mostrando o displayName de um Higher-Order Component](/assets/hoc-react/connectprint.PNG)

Vamos fazer as alterações no nosso HOC para aplicar essa convenção.

<div class="code javascript">
    <span class="file-name">createPopup.js</span>
<pre><code class="javascript">
import React from 'react';

const createPopup = Popup => {
   class CreatePopup extends React.Component{
      constructor(props){
         super(props);
      }

      closePopupHandler = () => {
         // lógica para fechar o popup
      }

      render(){
         // Aparentemente o highlightjs ainda não consegue lintar jsx :(

         // DESCONSIDEREM O COMENTÁRIO ABAIXO
         // return &lt;Popup closePopup={this.closePopupHandler} {...this.props} /&gt;;
      }
   }

   // Aqui definimos o nome que nosso componente receberá
   // seguindo a convenção Hoc(Componente)
   CreatePopup.displayName = `CreatePopup(${getDisplayName(Popup)})`;

   return CreatePopup;
}

function getDisplayName(Component){
   return Component.displayName || Component.name || "Component";
}

export default createPopup;

</code></pre>
</div>

Desse jeito o nome a ser mostrado no dev tools será, no caso do componente *SignInPopup*, **CreatePopup(SignInPopup)**.

#### Utilizar somente suas propriedades

Um ponto muito importante acerca dos **HOC's** é que para se manterem reutilizáveis e puros, eles não podem alterar **MUITO** as propriedades do componente que será *"incrementado"*, dessa forma é esperado que o componente gerado pelo **HOC** tenha comportamente semelhante ao do *componente original*.

No nosso exemplo não precisaremos aplicar essa convenção, mas ficaria algo semelhante a isso.

<div class="code javascript">
    <span class="file-name">UtilizoSoMinhasProps.js</span>
<pre><code>
import React from 'react';

function UtilizoSoMinhasProps(Componente){
   return class extends React.Component{
      constructor(props){
         super(props);
      }

      render(){
         // Aqui nós filtramos as propriedades que serão utilizadas somente
         //    nesse componente
         const { propExclusivaDoMeuHOC, ...outrasProps } = this.props;

         // nós fazemos algo com essa prop exclusiva
         const resultado = facoAlgoComAProp(propExclusivaDoMeuHOC);

         // e passamos o resultado como uma prop pro componente "temperado"
         return &lt;Popup propGerada={resultado} {...outrasProps} /&gt;;
      }
   }
} 

export default UtilizoSoMinhasProps;

</code></pre>
</div>

#### Permita que seus HOC's sejam composables

Acho que não tem tradução para o termo *composable*, de forma que o sentido **continue bom tecnicamente falando**, de qualquer maneira, para você que não entendeu o que eu quis dizer com isso, deixe que eu explico.

Você provavelmente ja deve conhecer *function composition*, que é basicamente uma função que recebe como parâmetro várias outras funções e retorna outra função que irá adiministrar o flow dos seus dados através das funções passadas anteriormente como parâmetro, se você não sabe, vou deixar [esse link](https://mathmesquita.me/2017/03/12/function-composition-no-javascript.html), você da uma lida lá e volta aqui depois.

Continuando, basicamente seu **HOC** será uma função correto? Se ele for um **HOC** simples como o do nosso exemplo, onde o único parâmetro passado é o componente que será *"temperado"*, então não precisamos esquentar muito a cabeça, ele ja está no que seria o **estágio final** de um **HOC** *composable*.

Agora em **HOC's** que você precisa passar mais parâmetros além do componente à ser *"temperado"*, ficando algo semelhante à isso aqui em baixo.

<div class="code javascript">
<pre><code>
const MeuComponenteComHoc = meuHOC(Componente, param1, param2);

</code></pre>
</div>

Essa função em uma linha pode até ficar normal de se ler, mas pense que em uma aplicação real você provavelmente terá outros HOC's que serão aplicados no seu componente. Um caso comum é o **connect** do react-redux, um *HOC* que é utilizado com frequência por quem utiliza redux para administrar o estado da sua aplicação.

<div class="code javascript">
<pre><code>
const MeuComponenteConectado = connect(mapStToProps, mapDpToProps)(Componente);

</code></pre>
</div>

Imagine você tendo que passar aquela linha toda dentro do parâmetro do connect.

<div class="code javascript">
<pre><code>
const MeuComponenteConectadoEComMeuHoc = connect(mapStToProps, mapDpToProps)(meuHOC(Componente, param1, param2));

</code></pre>
</div>

Não ficou muito legível não é mesmo? E é nessa hora que fazer **HOC's composables** irá nos ajudar, para transformá-lo em algo *composable*, teremos que imitar mais ou menos o **connect**, deixando nosso **HOC** assim.

<div class="code javascript">
<pre><code>
const MeuComponenteComHoc = meuHOC(param1, param2)(Componente);

</code></pre>
</div>

Se seu cérebro bugou com o que está acontecendo ali em cima tenha calma, uma outra forma de visualizar o que ocorreu é da seguinte forma.

<div class="code javascript">
<pre><code>
// meuHOC retornará uma função que será um HOC
const hoquizador = meuHOC(param1, param2);

// Assim passamos o componente para essa função como parâmetro
//    e ela tratará de adicionar as propriedades baseadas nos 
//    parâmetros passados acima
const MeuComponenteComHoc = hoquizador(Componente);

</code></pre>
</div>

A principio você pode achar que isso mais complicou do que ajudou, certo? Mas agora pense no caso do **connect**, com o nosso **HOC** funcionando dessa forma, poderiamos *compor* uma função que seria tipo um *"mega-HOC"* que iria passar nosso componente por todos os *HOC's* necessários.

<div class="code javascript">
<pre><code>
// Trocaremos isso
const MeuMegaComponente = connect(mapStToProps, mapDpToProps)(meuHOC(Componente, param1, param2));

// Por isso
// compose(a, b, c)(params) é o mesmo que a(b(c(params)))
const MegaHoc = compose(
   connect(mapStToProps, mapDpToProps),
   meuHoc(param1, param2)
);

const MeuMegaComponente = MegaHoc(Componente);
</code></pre>
</div>

Nosso código ficou bem mais legível não é ? E para quem ainda ficou com dúvidas sobre como seria a implementação desse **HOC composable**, o nosso *HOC* *meuHoc()*, teria o código semelhante a isso.

<div class="code javascript">
   <span class="file-name">meuHoc.js</span>
<pre><code>
import React from 'react';

// Nossa função aceita dois argumentos
function meuHoc(arg1, arg2){

   // Retorna outra função que será nosso HOC
   return function(Componente){

      // Como essa função é um HOC, retornaremos uma classe
      return class extends React.Component{
         constructor(props){
            super(props);
         }

         render(){
            // aqui por exemplo utilizariamos os argumentos da função
            //    "principal" para gerar as propriedades novas
            //    que serão adicionadas ao componente que passará pelo HOC
            let result1 = fazAlgoComArg(arg1);
            let result2 = fazAlgoComArg(arg2);

            // finalmente chamamos o Componente de forma normal
            //    passando como parametro o que fizemos com os argumentos
            //    que foram passados para nosso HOC
            return <Componente results={[result1, result2]} {...this.props} />
         }
      }
   }
}

export default meuHoc();
</code></pre>
</div>

### Conclusão

Vimos nesse post sobre os **HOC's** e como implementá-los da forma indicada para a maioria dos casos, o padrão de **Props Proxy**, que é quando nosso *HOC* irá utilizar as propriedades do *componente parâmetro* realizar alguma lógica nelas e aplicar esse resultado em outras propriedades do *componente* retornado.

Tem outro padrão de **HOC's** que é a **Inversão de Herança***(Inheritance Inversion)*, esse padrão faz com que o *componente à ser retornado* herde do *componente parâmetro*, existem algumas utilidades especificas para esse padrão de **HOC's**, talvez abordemos em outro post. Mas ja deixando claro aqui que ele não é indicado por que pode trazer problemas de performance no algoritmo de [reconciliação](https://facebook.github.io/react/docs/reconciliation.html) do React, a árvore de filhos pode não ser comparada corretamente.

Sei que a leitura ficou pesada com muito conteúdo, mas foi importante passar isso tudo para vocês, pois os *Higher-Order Components* facilitam muito nosso trabalho na hora de não sermos repetitivos.

Enfim galera, espero que vocês tenham gostado, obrigado pra quem leu até aqui, e qualquer dúvida, critica ou sugestão deixe-as nos comentários, responderei com um sorriso no rosto ! Obrigado novamente e até o próximo post !

**PS:** caso alguém conheça algum linter melhor que o highlight-js me fale por favor ! ficou bem complicado essas cores nos códigos. :(
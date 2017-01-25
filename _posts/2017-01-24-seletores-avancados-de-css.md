---
layout: post
title: "Seletores avançados de CSS #1"
image: "/assets/seletores-avancados-de-css/cover.jpg"
date:   2017-01-24 08:00:00 -0200
color_template: "css"
tags: css3 tutorial
resume: "O CSS tem muitas features que poucas pessoas conhecem, nesse post irei falar sobre os seletores avançados !"
topic: css
comments: true
---

<h3>Introdução</h3>
<p>Olá galera, como prometido no post do <a href="/2017/01/19/menu-toggle-sem-javascript">menu toggle sem javascript</a>, hoje irei falar sobre os seletores avançados do CSS. Pretendo fazer outro post somente com as <b>pseudo-classes</b> e <b>pseudo-elementos</b>.</p>
<p>A principio você pode me perguntar <i>"por que eu vou querer saber sobre seletores avançados?*</i>, mas acredite em mim, após você conhecê-los nunca mais irá querer parar de usá-los !</p>
<p>Sem mais delongas, mãos a obra !</p>

<h3>Seletores CSS</h3>
<p>Os seletores CSS são exatamente todas as keywords e métodos do CSS, inclusive '<i>.</i>' (ponto), '<i>#</i>' (hashtag) e a própria '<i>,</i>' (vírgula), sendo assim vou tentar "corrigir" um pouco o título, nós vamos falar somente dos seletores que não são nem pseudo-classes e nem pseudo-elementos.</p>
<p>Aqui segue uma listinha dos seletores que iremos abordar.</p>
<ul>
	<li><a href="#til">~</a></li>
	<li><a href="#maiorque">&gt;</a></li>
	<li><a href="#mais">+</a></li>
  <li><a href="#attr">[atributo]</a></li>
  <li><a href="#attrigual">[atributo=valor]</a></li>
  <li><a href="#attrmeio">[atributo~=valor]</a></li>
  <li><a href="#attrinicio">[atributo|=valor]</a></li>
  <li><a href="#attrinicio2">[atributo^=valor]</a></li>
  <li><a href="#attrfinal">[atributo$=valor]</a></li>
	<li><a href="#attrmeio2">[atributo*=valor]</a></li>
</ul>

<h4 id="til">elemento1 ~ elemento2</h4>
<p>O seletor <a target="_blank" href="http://www.w3schools.com/cssref/sel_gen_sibling.asp">til</a>, irá selecionar todos os próximos elementos no mesmo nível hierarquico do elemento1("origem").</p>
<div class="code html">
	<span class="file-name">index.html</span>
	<pre><code class="html">
&lt;!-- código CSS do snippet abaixo --&gt;
&lt;style&gt;
  ul li {
    padding: 10px 10px;
  }

  /* elemento que aplica til tem cor cinza claro*/
  ul li.til{
    background: #666;
    color: #fff;
  }

  /* ~ irá selecionar todos os elementos no mesmo nível
       hierarquico que tiverem a classe foo */
  ul li.til ~ .foo {
    background: #333;
    color: #fff;
  }
&lt;/style&gt;
...
...
&lt;!-- código html do snippet abaixo --&gt;
&lt;ul&gt;
  &lt;li class="til"&gt;elemento que aplica o til&lt;/li&gt;
  &lt;li&gt;elemento não afetado&lt;/li&gt;
  &lt;li class="foo"&gt;elemento que sofre efeito do til&lt;/li&gt;
  &lt;li class="foo"&gt;elemento que sofre efeito do til&lt;/li&gt;
  &lt;li&gt;elemento não afetado&lt;/li&gt;
  &lt;li&gt;elemento não afetado&lt;/li&gt;
  &lt;li class="foo"&gt;elemento que sofre efeito do til&lt;/li&gt;
  &lt;ul&gt;
    &lt;li&gt;elemento em outro nivel não afetado&lt;/li&gt;
    &lt;li class="foo"&gt;elemento em outro nivel com classe foo não alterado&lt;/li&gt;
  &lt;/ul&gt;
&lt;/ul&gt;
</code></pre>
</div>
<p data-height="454" data-theme-id="0" data-slug-hash="vgJLJJ" data-default-tab="result" data-user="mathmesquita" data-embed-version="2" data-pen-title="seletor til" class="codepen">See the Pen <a target="_blank" href="http://codepen.io/mathmesquita/pen/vgJLJJ/">seletor til</a> by Matheus Mesquita (<a target="_blank" href="http://codepen.io/mathmesquita">@mathmesquita</a>) on <a target="_blank" href="http://codepen.io">CodePen</a>.</p>
<script async src="https://production-assets.codepen.io/assets/embed/ei.js"></script>

<h4 id="maiorque">elemento1 > elemento2</h4>
<p>O seletor <a target="_blank" href="http://www.w3schools.com/cssref/sel_element_gt.asp">></a>, irá selecionar somente os elementos <i>filhos de primeiro grau</i> do elemento1("origem").</p>
<div class="code html">
	<span class="file-name">index.html</span>
	<pre><code class="html">
&lt;!-- código CSS do snippet abaixo --&gt;
&lt;style&gt;
  *{font-family: Verdana;}
  ul li {
    padding: 10px 10px;
  }

  /* > irá selecionar todos os elementos filhos de 
       primeiro grau que tenham a classe .foo */
  ul.maiorque > li.foo{
    background: #666;
    color: #fff;
  }
&lt;/style&gt;
...
...
&lt;!-- código html do snippet abaixo --&gt;
&lt;ul class="maiorque"&gt;
  &lt;li class="foo"&gt;filho de primeiro grau&lt;/li&gt;
  &lt;li&gt;filho de primeiro grau sem classe .foo&lt;/li&gt;
  &lt;li class="foo"&gt;filho de primeiro grau&lt;/li&gt;
  &lt;li&gt;filho de primeiro grau sem classe .foo&lt;/li&gt;
  &lt;li class="foo"&gt;filho de primeiro grau&lt;/li&gt;
  &lt;ul&gt;
    &lt;li class="foo"&gt;filho de segundo grau&lt;/li&gt;
    &lt;li class="foo"&gt;filho de segundo grau&lt;/li&gt;
    &lt;ul&gt;
      &lt;li class="foo"&gt;filho de terceiro grau&lt;/li&gt;
      &lt;li class="foo"&gt;filho de terceiro grau&lt;/li&gt;
    &lt;/ul&gt;
  &lt;/ul&gt;
&lt;/ul&gt;
</code></pre>
</div>
<p data-height="439" data-theme-id="0" data-slug-hash="Ndvxze" data-default-tab="result" data-user="mathmesquita" data-embed-version="2" data-pen-title="seletor maior que" class="codepen">See the Pen <a target="_blank" href="http://codepen.io/mathmesquita/pen/Ndvxze/">seletor maior que</a> by Matheus Mesquita (<a target="_blank" href="http://codepen.io/mathmesquita">@mathmesquita</a>) on <a target="_blank" href="http://codepen.io">CodePen</a>.</p>

<h4 id="mais">elemento1 + elemento2</h4>
<p>O seletor <a target="_blank" href="http://www.w3schools.com/cssref/sel_element_pluss.asp">+</a>, é parecido com o <b>~</b>, a diferença é que ele selecionará somente o elemento2 que esteja <b>IMEDIATAMENTE</b> após o elemento1("origem") no mesmo nível hierarquico.</p>
<div class="code html">
  <span class="file-name">index.html</span>
  <pre><code class="html">
&lt;!-- código CSS do snippet abaixo --&gt;
&lt;style&gt;
  *{font-family: Verdana;}
  ul li {
    padding: 10px 10px;
    margin: 5px 0;
  }

  ul li.mais{
    background: #666;
    color: #fff;
  }
  ul li.foo{
    background: #eee;
    color: #777;
  }
  /* + irá selecionar o elemento imediatamente após
       o elemento .mais e que tenha a classe .foo */
  ul li.mais + .foo {
    background: #333;
    color: #fff;
  }
&lt;/style&gt;
...
...
&lt;!-- código html do snippet abaixo --&gt;
&lt;ul&gt;
  &lt;li class="mais"&gt;elemento que aplica o mais&lt;/li&gt;
  &lt;li class="foo"&gt;elemento que sofre efeito do mais&lt;/li&gt;
  &lt;li class="foo"&gt;elemento não afetado, mesmo com classe foo&lt;/li&gt;
  &lt;li class="foo"&gt;elemento não afetado, mesmo com classe foo&lt;/li&gt;
  &lt;li&gt;elemento sem classe foo&lt;/li&gt;
  &lt;li class="mais"&gt;elemento que aplica o mais&lt;/li&gt;
  &lt;li&gt;elemento sem classe foo(imediatamente após)&lt;/li&gt;
  &lt;li class="foo"&gt;elemento não afetado, mesmo com classe foo&lt;/li&gt;
  &lt;ul&gt;
    &lt;li&gt;elemento sem classe foo em outro nivel&lt;/li&gt;
    &lt;li class="foo"&gt;elemento não afetado, mesmo com classe foo&lt;/li&gt;
  &lt;/ul&gt;
&lt;/ul&gt;
</code></pre>
</div>
<p data-height="265" data-theme-id="0" data-slug-hash="NdvNKV" data-default-tab="result" data-user="mathmesquita" data-embed-version="2" data-pen-title="seletor mais" class="codepen">See the Pen <a target="_blank" href="http://codepen.io/mathmesquita/pen/NdvNKV/">seletor mais</a> by Matheus Mesquita (<a target="_blank" href="http://codepen.io/mathmesquita">@mathmesquita</a>) on <a target="_blank" href="http://codepen.io">CodePen</a>.</p>

<h3>Seletores de atributos</h3>
<p>Para ficar mais facil a visualização, eu primeiro separei todas variedades do seletor de atributos e por fim terá o exemplo de todos pelo codepen !</p>

<h4 id="attr">[atributo]</h4>
<p>O seletor <a target="_blank" href="http://www.w3schools.com/cssref/sel_attribute.asp">[atributo]</a>, irá selecionar todos os elementos que possuam aquele atributo, independente de ter conteúdo.</p>
<div class="code css">
  <span class="file-name">index.css</span>
  <pre><code>
img[alt]{
  /* estilo */
}
</code></pre>
</div>

<h4 id="attrigual">[atributo=valor]</h4>
<p>O seletor <a target="_blank" href="http://www.w3schools.com/cssref/sel_attribute_value.asp">[atributo=valor]</a>, irá selecionar todos os elementos que tenham aquele atributo exatamente igual ao valor passado.</p>
<div class="code css">
  <span class="file-name">index.css</span>
  <pre><code class="css" >
input[type="checkbox"]{
  /* estilo */
}
</code></pre>
</div>

<h4 id="attrmeio">[atributo~=valor]</h4>
<p>O seletor <a target="_blank" href="http://www.w3schools.com/cssref/sel_attribute_value_contains.asp">[atributo~=valor]</a>, irá selecionar todos os elementos que o atributo tenha a palavra passada como valor, <b>tem que ser a palavra passada sozinha, não pega os caracteres, mas sim a palavra</b>.</p>
<div class="code css">
  <span class="file-name">index.css</span>
  <pre><code class="css" >
a[title~="mathmesquita"]{
  /* estilo */
}
</code></pre>
</div>

<h4 id="attrinicio">[atributo|=valor]</h4>
<p>O seletor <a target="_blank" href="http://www.w3schools.com/cssref/sel_attribute_value_lang.asp">[atributo|=valor]</a>, irá selecionar todos os elementos que o valor do atributo <b>comece</b> com a palavra passada, <b>separada da próxima palavra por hífen</b>.</p>
<div class="code css">
  <span class="file-name">index.css</span>
  <pre><code class="css" >
a[href|="https//"]{
  /* estilo */
}
</code></pre>
</div>

<h4 id="attrinicio2">[atributo^=valor]</h4>
<p>O seletor <a target="_blank" href="http://www.w3schools.com/cssref/sel_attr_begin.asp">[atributo^=valor]</a>, irá funcionar da mesma forma que <b>[atributo|=valor]</b>, selecionando todos os elementos que o atributo <b>comece</b> com o valor passado.</p>
<div class="code css">
  <span class="file-name">index.css</span>
  <pre><code class="css">
[class^="gallery"]{
  /* estilo */
}
</code></pre>
</div>

<h4 id="attrfinal">[atributo$=valor]</h4>
<p>O seletor <a target="_blank" href="http://www.w3schools.com/cssref/sel_attr_end.asp">[atributo$=valor]</a>, irá selecionar todos os elementos que o atributo <b>termine</b> com o valor passado.</p>
<div class="code css">
  <span class="file-name">index.css</span>
  <pre><code class="css" >
a[href$=".pdf"]{
  /* estilo */
}
</code></pre>
</div>

<h4 id="attrmeio2">[atributo*=valor]</h4>
<p>O seletor <a target="_blank" href="http://www.w3schools.com/cssref/sel_attr_contain.asp">[atributo*=valor]</a>, irá funcionar da mesma forma que <b>[atributo~=valor]</b>, selecionando todos os elementos que o atributo possua a string passada em valor</p>
<div class="code css">
  <span class="file-name">index.css</span>
  <pre><code class="css" >
img[alt*="montanhas"]{
  /* estilo */
}
</code></pre>
</div>

<h4>Exemplos</h4>
<div class="code html">
  <span class="file-name">index.html</span>
  <pre><code class="html">
&lt;!-- código CSS do snippet abaixo --&gt;
&lt;style&gt;
  *{font-family: Verdana;}

  /* seleciona todas as imagems com atributo alt e bota borda azul */
  img[alt]{
    border: 4px solid blue;
  }

  /* seleciona todos os inputs com type checkbox e os aumenta*/
  input[type="checkbox"]{
    width: 100px;
    height: 100px;
  }

  /* seleciona todos os links que tenham a palavra mathmesquita no atributo title e deixa a cor da letra vermelha */
  a[title~='mathmesquita']{
    color: red;
  }

  /* seleciona todos os links que comecem com a palavra 'https', separada da próxima palavra por hífen, no atributo title e deixa com o fundo preto */
  a[title|="https"]{
    background: #000;
    color: #fff;
  }

  /* seleciona todos os elementos do DOM que possuem a classe começando com a string 'gallery' e adiciona uma borda verde*/
  [class^="gallery"]{
    border: 3px solid green;
  }

  /* seleciona todos os links que o atributo href termina com .pdf e deixa a cor vermelha com fundo preto */
  a[href$=".pdf"]{
    color: red;
    background: #000;
  }

  /* seleciona todas as imagems que possuem a palavra montanhas no seu atributo alt e deixa a cor da borda amarela*/
  img[alt*="montanhas"]{
    border-color: yellow;
  }
&lt;/style&gt;
...
...
&lt;!-- código html do snippet abaixo --&gt;
&lt;img src="https://image.freepik.com/icones-gratis/facebook_318-136394.jpg" alt="Logo do facebook" /&gt;

&lt;br /&gt;&lt;br /&gt;
&lt;input type="checkbox" /&gt;

&lt;br /&gt;&lt;br /&gt;
&lt;a href="http://mathmesquita.me/" title="link para o site do mathmesquita"&gt;link para meu blog com title contendo mathmesquita&lt;/a&gt;

&lt;br /&gt;&lt;br /&gt;
&lt;a href="https://mathmesquita.me/" title="https-é indicado para o meu blog"&gt;link para meu blog com title começando com https&lt;/a&gt;

&lt;br /&gt;&lt;br /&gt;
&lt;div class="galleryBig"&gt;
  div começando com classe gallery
&lt;/div&gt;
&lt;img class="galleryItem" src="http://www.mrpatel.net/IMAGES/JavaScript.png" /&gt;
&lt;div&gt;
  imagem com classe galleryItem e sem atributo alt
&lt;/div&gt;

&lt;br /&gt;&lt;br /&gt;
&lt;a href="http://www.yugioh-card.com/ygo_cms/ygo/all/uploads/Rulebook_v9_pt.pdf" target="_blank"&gt;link para um arquivo .pdf&lt;/a&gt;

&lt;br /&gt;&lt;br /&gt;
&lt;img src="https://image.freepik.com/icones-gratis/paisagem-das-montanhas-no-dia-ensolarado_318-74296.jpg" alt="icone com montanhas e um sol" /&gt;
</code></pre>
</div>
<p data-height="508" data-theme-id="0" data-slug-hash="bgrwYx" data-default-tab="result" data-user="mathmesquita" data-embed-version="2" data-pen-title="seletores [atributo [=valor]]" class="codepen">See the Pen <a target="_blank" href="http://codepen.io/mathmesquita/pen/bgrwYx/">seletores [atributo [=valor]]</a> by Matheus Mesquita (<a target="_blank" href="http://codepen.io/mathmesquita">@mathmesquita</a>) on <a target="_blank" href="http://codepen.io">CodePen</a>.</p>


<h3>Conclusão</h3>
<p>Seletores avançados são muito úteis quando queremos fazer as coisas da maneira mais simples e rápida, sem pensar em muitas classes ou qualquer padrão de escrita de CSS como <a target="_blank" href="http://getbem.com/introduction/">BEM</a>, <a target="_blank" href="http://itcss.io/">ITCSS</a> etc.</p>
<p>Existem outros seletores que chamamos de <b>pseudo-classes</b> e <b>pseudo-elementos</b> que não foram abordados nesse post e que também são muito úteis para truques de CSS, desenhos e muito mais coisa, esperem o próximo post que irei falar de todos eles !</p>
<p>Espero que tenham gostado, e obrigado por quem leu até aqui, dúvidas, críticas e sugestões, os comentários estão sempre abertos! Por hoje é só isso e até a próxima galera. :D</p>
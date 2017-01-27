---
layout: post
title: "Seletores avançados de CSS #2"
image: "/assets/seletores-avancados-de-css/cover2.jpg"
date:   2017-01-25 08:00:00 -0200
color_template: "css"
tags: css3 tutorial
resume: "Você conhece as pseudo-classes do CSS? não faz idéia ? No post de hoje da série iremos falar sobre elas (ou da maioria delas)..."
topic: css
comments: true
---

<h3>Introdução</h3>
<p>Fala pessoal, sejam bem-vindos ao segundo post da série que irá te ensinar de uma vez por todas sobre todos os seletores de CSS.</p>
<p>No post de hoje iremos falar sobre as <b>pseudo-classes</b>, ja avisando que não conseguiremos abordar todas pois são muitas, mas nada ficará deixado para trás e tudo será mencionado nos próximos episódios dessa série.</p>
<p>Temos muitas <b>pseudo-classes</b> e pouco tempo, vamos tacar o pau nesse carrinho !</p>

<h3>pseudo-classes</h3>
<p>As <p>pseudo-classes</p> do css são todas as propriedades que começam com :<i>(dois pontos)</i>, tipo os mais comuns como <i>:hover</i>, <i>:active</i> e <i>:focus</i>, apesar de geralmente estarem acompanhadas de outros seletores normais tipo classes, ids ou elementos, elas também podem ser usadas separadamente, como o exemplo do <i>:root</i>.</p>
<p>No post de hoje, nós iremos ver as seguintes <b>pseudo-classes</b>:</p>
<ul>
	<li><a href="#active">:active</a></li>
	<li><a href="#empty">:empty</a></li>
	<li><a href="#hover">:hover</a></li>
	<li><a href="#checked">:checked</a></li>
	<li><a href="#disabled">:disabled</a></li>
	<li><a href="#enabled">:enabled</a></li>
	<li><a href="#focus">:focus</a></li>
  <li><a href="#invalid">:invalid</a></li>
	<li><a href="#valid">:valid</a></li>
  <li><a href="#inrange">:in-range</a></li>
	<li><a href="#outofrange">:out-of-range</a></li>
	<li><a href="#optional">:optional</a></li>
  <li><a href="#required">:required</a></li>
  <li><a href="#readonly">:read-only</a></li>
	<li><a href="#read-write">:read-write</a></li>
</ul>

<h4 id="active">:active</h4>
<p>A classe <a href="http://www.w3schools.com/cssref/sel_active.asp">:active</a>, irá estilizar o elemento no momento em que ele for ativado, todo elemento recebe o status de <i>:active</i> <b>quando são clicados</b>.</p>
<div class="code html">
	<span class="file-name">index.html</span>
	<pre><code class="html">
&lt;!-- código CSS do snippet abaixo --&gt;
&lt;style&gt;
/* todos os elementos div, p e button quando 
   forem clicados ficarão da cor azul e fonte branca */
  div:active, p:active, button:active {
    background-color: blue;
    color: #fff;
  }
&lt;/style&gt;
...
...
&lt;!-- código html do snippet abaixo --&gt;
&lt;div&gt;
Conteudo dentro da div para ela ter uma altura
&lt;/div&gt;

&lt;p&gt;Texto dentro da tag p&lt;/p&gt;

&lt;button&gt;Um botão&lt;/button&gt;
...
</code></pre>
</div>
<p data-height="187" data-theme-id="0" data-slug-hash="MJEBXr" data-default-tab="result" data-user="mathmesquita" data-embed-version="2" data-pen-title="Pseudo-classe :active" class="codepen">See the Pen <a href="http://codepen.io/mathmesquita/pen/MJEBXr/">Pseudo-classe :active</a> by Matheus Mesquita (<a href="http://codepen.io/mathmesquita">@mathmesquita</a>) on <a href="http://codepen.io">CodePen</a>.</p>
<script async src="https://production-assets.codepen.io/assets/embed/ei.js"></script>

<h4 id="empty">:empty</h4>
<p>A classe <a href="http://www.w3schools.com/cssref/sel_empty.asp">:empty</a>, irá adicionar estilo a todos os elementos que não tiverem conteúdo nenhum. <b>Literalmente nenhum, não pode nem ter "pulo de linha" ou espaço.</b></p>
<div class="code html">
  <span class="file-name">index.html</span>
  <pre><code class="html">
&lt;!-- código CSS do snippet abaixo --&gt;
&lt;style&gt;
/* todos os elementos que estiverem totalmente vazios 
   inclusive de "pulos de linha", ganha um tamanho para 
   ser visivel e cor de fundo azul */
  div:empty, p:empty, button:empty{
    background-color: blue;
    height: 10px;
    width: 100px;
  }
&lt;/style&gt;
...
...
&lt;!-- código html do snippet abaixo --&gt;
&lt;div&gt;&lt;/div&gt;

&lt;p&gt;um p com conteudo&lt;/p&gt;
&lt;p&gt;&lt;/p&gt;

&lt;button&gt;&lt;/button&gt;
...
</code></pre>
</div>
<p data-height="182" data-theme-id="0" data-slug-hash="PWJBdZ" data-default-tab="result" data-user="mathmesquita" data-embed-version="2" data-pen-title="Pseudo-classe :empty" class="codepen">See the Pen <a href="http://codepen.io/mathmesquita/pen/PWJBdZ/">Pseudo-classe :empty</a> by Matheus Mesquita (<a href="http://codepen.io/mathmesquita">@mathmesquita</a>) on <a href="http://codepen.io">CodePen</a>.</p>

<h4 id="hover">:hover</h4>
<p>A classe <a href="http://www.w3schools.com/cssref/sel_hover.asp">:hover</a>, quando o mouse passar por cima do elemento, ele receberá a classe <i>:hover</i>.</p>
<div class="code html">
  <span class="file-name">index.html</span>
  <pre><code class="html">
&lt;!-- código CSS do snippet abaixo --&gt;
&lt;style&gt;
/* todos os elementos que o mouse passar por cima
  receberão a pseudo-classe hover, passando a ficar
  com fundo azul e letras brancas */
  div:hover, p:hover, button:hover{
    background-color: blue;
    color: #fff;
  }
&lt;/style&gt;
...
...
&lt;!-- código html do snippet abaixo --&gt;
&lt;div&gt;div com hover&lt;/div&gt;

&lt;p&gt;um p com hover&lt;/p&gt;

&lt;button&gt;botão com hover&lt;/button&gt;
...
</code></pre>
</div>
<p data-height="265" data-theme-id="0" data-slug-hash="wgrEor" data-default-tab="result" data-user="mathmesquita" data-embed-version="2" data-pen-title="Pseudo-classe :hover" class="codepen">See the Pen <a href="http://codepen.io/mathmesquita/pen/wgrEor/">Pseudo-classe :hover</a> by Matheus Mesquita (<a href="http://codepen.io/mathmesquita">@mathmesquita</a>) on <a href="http://codepen.io">CodePen</a>.</p>

<h4 id="checked">:checked</h4>
<p>A classe <a href="http://www.w3schools.com/cssref/sel_checked.asp">:checked</a>, irá estilizar todos os elementos <i>input</i> do tipo <i>checkbox</i> ou <i>radio</i>, que estiverem marcados.</p>
<div class="code html">
  <span class="file-name">index.html</span>
  <pre><code class="html">
&lt;!-- código CSS do snippet abaixo --&gt;
&lt;style&gt;
/* todos os labels que vem imediatamente 
   após os inputs marcados(selecionados), 
   ficarão com fundo azul e texto branco */
  input:checked + label{
    background-color: blue;
    color: #fff;
  }
&lt;/style&gt;
...
...
&lt;!-- código html do snippet abaixo --&gt;
&lt;input type="checkbox" id="checkbox1"/&gt;
&lt;label for="checkbox1"&gt;A checkbox 1&lt;/label&gt;
&lt;br /&gt;
&lt;input type="checkbox" id="checkbox2"/&gt;
&lt;label for="checkbox2"&gt;A checkbox 2&lt;/label&gt;
&lt;br /&gt;
&lt;input type="radio" name="rad" id="radio1" /&gt;
&lt;label for="radio1"&gt;Radio 1&lt;/label&gt;
&lt;br /&gt;
&lt;input type="radio" name="rad" id="radio2" /&gt;
&lt;label for="radio2"&gt;Radio 2&lt;/label&gt;
...
</code></pre>
</div>
<p data-height="158" data-theme-id="0" data-slug-hash="ygzxRw" data-default-tab="result" data-user="mathmesquita" data-embed-version="2" data-pen-title="Pseudo-classe :checked" class="codepen">See the Pen <a href="http://codepen.io/mathmesquita/pen/ygzxRw/">Pseudo-classe :checked</a> by Matheus Mesquita (<a href="http://codepen.io/mathmesquita">@mathmesquita</a>) on <a href="http://codepen.io">CodePen</a>.</p>

<h3>Pseudo-classes de Inputs</h3>
<p>Tirando os inputs de tipo <b>checkbox</b> e o <b>radio</b>, resolvi juntar todos os outros da mesma forma que eu fiz com o seletor [attr] no outro post. Sendo assim todos os exemplos irão aparecer após as <i>pseudo-classes</i> e suas explicações.</p>

<h4 id="disabled">:disabled</h4>
<p>A classe <a href="http://www.w3schools.com/cssref/sel_disabled.asp">:disabled</a>, irá estilizar todos os elementos do tipo <i>input</i> que possuirem o atributo <i>disabled</i>.</p>

<h4 id="enabled">:enabled</h4>
<p>A classe <a href="http://www.w3schools.com/cssref/sel_enabled.asp">:enabled</a>, irá estilizar ao contrário da <b>:disabled</b> todos os elementos sem o atributo <i>disabled</i>.</p>

<h4 id="focus">:focus</h4>
<p>A classe <a href="http://www.w3schools.com/cssref/sel_focus.asp">:focus</a>, irá adicionar estilo a todos os elementos do tipo <b>input</b> que estiverem com o foco do usuário.</p>

<h4 id="invalid">:invalid</h4>
<p>A classe <a href="http://www.w3schools.com/cssref/sel_invalid.asp">:invalid</a>, irá estilizar todos os elementos que estiverem inválidos para o tipo do <b>input</b>.</p>

<h4 id="valid">:valid</h4>
<p>A classe <a href="http://www.w3schools.com/cssref/sel_valid.asp">:valid</a>, irá estilizar os <b>inputs</b> que estiverem válidos.</p>

<h4 id="inrange">:in-range</h4>
<p>A classe <a href="http://www.w3schools.com/cssref/sel_in-range.asp">:in-range</a>, irá adicionar estilo nos <b>inputs</b> que estiverem dentro dos limites estabelecidos por <i>min</i> e <i>max</i>.</p>

<h4 id="outofrange">:out-of-range</h4>
<p>A classe <a href="http://www.w3schools.com/cssref/sel_out-of-range.asp">:out-of-range</a>, irá adicionar estilo nos <b>inputs</b> que <b>NÃO</b> estiverem dentro dos limites estabelecidos por <i>min</i> e <i>max</i>.</p>

<h4 id="optional">:optional</h4>
<p>A classe <a href="http://www.w3schools.com/cssref/sel_optional.asp">:optional</a>, irá estilizar todos os <b>inputs</b> que <b>NÃO</b>, receberam o atributo <i>required</i>.</p>

<h4 id="required">:required</h4>
<p>A classe <a href="http://www.w3schools.com/cssref/sel_required.asp">:required</a>, irá estilizar todos os <b>inputs</b> que possuirem a tag <i>required</i>.</p>

<h4 id="readonly">:read-only</h4>
<p>A classe <a href="http://www.w3schools.com/cssref/sel_read-only.asp">:read-only</a>, irá estilizar todos os <b>inputs</b> que tiverem a tag <i>readonly</i>.</p>

<h4 id="readwrite">:read-write</h4>
<p>A classe <a href="http://www.w3schools.com/cssref/sel_read-write.asp">:read-write</a>, irá adicionar estilo a todos os <b>inputs</b> que <b>NÃO</b> tiverem a tag <i>readonly</i>.</p>

<h4>Exemplos</h4>
<p>É pessoal, como viram, tem muitas pseudo-classes possíveis em elementos do tipo input chegando ao ponto de algumas delas vocês talvez nunca irão usar, mas é claro que com criatividade todas podem ser usadas ! <b>LEMBRANDO QUE</b>, <i>algumas dessas pseudo-classes foram introduzidas no css3 e para ter compartibilidade em todos os navegadores elas possuem alguns prefixos, dessa maneira então é sempre indicado você passar o seu CSS em um auto-prefixer</i>.</p>
<div class="code html">
  <span class="file-name">index.html</span>
  <pre><code class="html">
&lt;!-- código CSS do snippet abaixo --&gt;
&lt;style&gt;
label{font-family: Verdana, sans-serif;font-size: 14px;}

/* inputs desabilitados ficarão com fundo  
   vermelho e bordas pretas*/
  input:disabled{
    background-color: red;
    border: 1px solid #000;
  }

/* o input com id #ex2 que estiver habilitado 
   ficaá com cor de fundo amarela e letras pretas */
  input#ex2:enabled{
    background-color: yellow;
    color: #000;
  }

/* o input com o id #ex3 que estiver com foco do usuario 
   ficará com uma borda de 2px azul*/
  input#ex3:focus{
    border: 2px solid blue;
  }

/* o input com id #ex4 com conteudo invalido 
   terá uma borda vermelha */
  input#ex4:invalid{
    border-color: red;
  }

/* o input com id #ex5 valido ficará com fundo 
   verde e letras brancas */
  input#ex5:valid{
    background-color: green;
    color: #fff;
  }

/* o input que estiver dentro do range irá ter 
   borda verde */
  input:in-range{
    border-color: green;
  }

/* o input que estiver fora do range ficará com 
   fundo rosa */
  input:out-of-range{
    background-color: pink;
  }

/* o input com id #ex8 e que for opcional ficará 
   com fundo verde escuro e letras brancas */
  input#ex8:optional{
    background-color: #003300;
    color: #fff;
  }

/* o input que for obrigatório 
   ficará com fundo vermelho escuro e letras brancas */
  input:required {
    background-color: #660000;
    color: #fff;
  }

/* o input que estiver disponivel somente para leitura 
   ficará com fundo cinza e borda cinza escuro */
  input:read-only{
    background: #e3e3e3;
    border: 1px solid #666;
  }

/* o input com id #ex11 e disponivel para edição sem a tag
   readonly ficará com fundo laranja e letras brancas*/
  input#ex11:read-write{
    background: #ff3300;
    color: #fff;
  }
&lt;/style&gt;
...
...
&lt;!-- código html do snippet abaixo --&gt;
&lt;label for="ex1"&gt;Input :disabled&lt;/label&gt;&lt;br /&gt;
&lt;input id="ex1" type="text" disabled /&gt;
&lt;br /&gt;&lt;br /&gt;

&lt;label for="ex2"&gt;Input :enabled&lt;/label&gt;&lt;br /&gt;
&lt;input id="ex2" type="text" /&gt;
&lt;br /&gt;&lt;br /&gt;

&lt;label for="ex3"&gt;Clique no input para visualizar o :focus&lt;/label&gt;&lt;br /&gt;
&lt;input id="ex3" type="text" /&gt;
&lt;br /&gt;&lt;br /&gt;

&lt;label for="ex4"&gt;Input de email invalido&lt;/label&gt;&lt;br /&gt;
&lt;input id="ex4" type="email" value="meu_email_mal_formatado"/&gt;
&lt;br /&gt;&lt;br /&gt;

&lt;label for="ex5"&gt;Input de email valido&lt;/label&gt;&lt;br /&gt;
&lt;input id="ex5" type="email" value="meu_email@gmail.com"/&gt;
&lt;br /&gt;&lt;br /&gt;

&lt;label for="ex6"&gt;Input que está dentro dos limites&lt;/label&gt;&lt;br /&gt;
&lt;input id="ex6" type="number" min="0" max="100" value="36" /&gt;
&lt;br /&gt;&lt;br /&gt;

&lt;label for="ex7"&gt;Input que está FORA dos limites&lt;/label&gt;&lt;br /&gt;
&lt;input id="ex7" type="number" min="0" max="100" value="402" /&gt;
&lt;br /&gt;&lt;br /&gt;

&lt;label for="ex8"&gt;Input opcional&lt;/label&gt;&lt;br /&gt;
&lt;input id="ex8" type="text" /&gt;
&lt;br /&gt;&lt;br /&gt;

&lt;label for="ex9"&gt;Input obrigatório&lt;/label&gt;&lt;br /&gt;
&lt;input id="ex9" type="text" required /&gt;
&lt;br /&gt;&lt;br /&gt;

&lt;label for="ex10"&gt;Input disponível somente para leitura :read-only&lt;/label&gt;&lt;br /&gt;
&lt;input id="ex10" type="text" value="esse post é demais" readonly /&gt;
&lt;br /&gt;&lt;br /&gt;

&lt;label for="ex11"&gt;Input input que pode ser alterado e lido :read-write&lt;/label&gt;&lt;br /&gt;
&lt;input id="ex11" type="text" value="você pode alterar isso" /&gt;
...
</code></pre>
</div>
<p data-height="265" data-theme-id="0" data-slug-hash="oBGaRy" data-default-tab="result" data-user="mathmesquita" data-embed-version="2" data-pen-title="Pseudo-classes inputs" class="codepen">See the Pen <a href="http://codepen.io/mathmesquita/pen/oBGaRy/">Pseudo-classes inputs</a> by Matheus Mesquita (<a href="http://codepen.io/mathmesquita">@mathmesquita</a>) on <a href="http://codepen.io">CodePen</a>.</p>

<h4>Conclusão</h4>
<p>Bem galera, sempre bom lembrar, algumas dessas <i>pseudo-classes</i> são dependentes de prefixos, sendo assim sempre passe seu código em um auto-prefixer !</p>
<p>O post ficou longo, especialmente pela tag <b>input</b> que tem várias pseudo-classes muito interessantes e que podem ser usadas no nosso dia a dia para informar ao usuário o que está acontecendo na interface dele. </p>
<p>Por hoje é só<strike>(foi coisa pra krl)</strike>, e espero que tenham gostado, muito obrigado a quem ficou lendo até aqui e espero comentários críticas e sugestões como sempre ! Valeu de novo e até o próximo post. :D</p>
---
layout: post
title: "Menu toggle sem javascript"
image: "/assets/menu-sem-javascript/cover.jpg"
date:   2017-01-19 08:00:00 -0200
color_template: "css"
tags: CSS tutorial inspiracao
resume: "Seletores avançados servem para muitas coisas e hoje vamos ver um dos usos mais legais deles."
topic: css
comments: true
---

<h3>Introdução</h3>
<p>Fala galera, hoje irei ensinar a fazer um menu parecido com o que eu tenho aqui no blog, usando somente seletores avançados, <b>assim como o meu</b>.</p>
<p>A compatibilidade é boa, podemos utilizar essa técnica a partir do IE9, segundo o <a href="http://caniuse.com/#feat=css-sel3">caniuse</a>, abaixo disso será necessário utilizar JS. =/</p>
<p>Sem mais, bora começar nosso menu.</p>

<h3>Como vamos fazer isso?</h3>
<h4>html</h4>
<p>Primeiramente devemos definir nossa estrutura no html, o truque está todo na posição dos elementos que iremos criar.</p>
<p>Prestem atenção nessa estrutura:</p>
<div class="code html">
	<span class="file-name">index.html</span>
	<pre><code>
...
&lt;!-- O checkbox é o principal item dessa nossa brincadeira
		graças ao seletor :checked nós poderemos ver se ele 
		está ativo ou não --&gt;
&lt;input type="checkbox" id="menu-tgl" /&gt;

&lt;!-- Aqui vem o menu que ficará escondido --&gt;
&lt;nav class="menu"&gt;
	&lt;!-- Nosso outro protagonista, a tag label, será
			responsável por ativar ou desativar o nosso checkbox
			ativando e desativando a pseudo-classe :checked  --&gt;
	&lt;label for="menu-tgl" class="menu-btn"&gt;&lt;/label&gt;
	
	&lt;!-- Conteúdo do nosso menu --&gt;
	&lt;ul&gt;
		&lt;li&gt;&lt;a href="#"&gt;Página 1&lt;/a&gt;&lt;/li&gt;
		&lt;li&gt;&lt;a href="#"&gt;Página 2&lt;/a&gt;&lt;/li&gt;
		&lt;li&gt;&lt;a href="#"&gt;Página 3&lt;/a&gt;&lt;/li&gt;
	&lt;/ul&gt;
&lt;/nav&gt;
...
</code></pre>
</div>

<h4>css</h4>
<p>No nosso arquivo CSS nós faremos a magica acontecer, usando o <i>:checked</i> e <i>~</i>, conseguiremos acessar os próximos elementos da árvore DOM e mudar seu estilo baseado no checkbox estar ativo ou inativo.</p>
<div class="code css">
	<span class="file-name">index.css</span>
	<pre><code>
/* importante resetar os valores default de margin e padding 
   de todos os elementos, exitem outras propriedades também
   que são boas de serem resetadas, indico importarem o arquivo 
   http://meyerweb.com/eric/tools/css/reset/reset.css antes de todo projeto
   como nosso caso é simples, isso não será necessário */
*{margin:0;padding:0;}

/* o nosso checkbox não precisa estar visível para que possamos
   acessar sua pseudo-classe :checked, sendo assim nós definimos
   o display como none, que vai funcionar como se o nosso elemento
   nem estivesse no DOM, não irá ocupar nenhum espaço, ao contrário
    do que seria se simplesmente definissemos opacity:0 */
#menu-tgl {
	display: none;
}

/* aqui aparece o primeiro uso do nosso seletor ~, esse seletor
   é usado quando queremos selecionar os próximos elementos na
   árvore, desde que estejam no mesmo nivel, no nosso caso o
   input[type=checkbox] está na mesma hierarquia que a nossa nav.menu */
#menu-tgl ~ .menu {
	box-sizing: border-box;
	position: fixed;
	width: 350px;
	height: 100%;
	background: #101925;
	font-family: Verdana;
	padding-top: 20px;
	padding-left: 40px;
	transform: translateX(-100%);
	transition: transform 0.7s ease;
}
/* pre-fixos: eu não estou utilizando nenhum pré-fixo nesse arquivo
   para não deixar muito longo o nosso post(mais do que ja está) */

/* aqui usamos novamente o seletor ~, se olharem direito
   como nosso label.menu-btn está dentro da nossa nav.menu
   só conseguimos acessa-la se formos pelo nav.menu que está
   na mesma hierarquia que nosso input#menu-tgl */
#menu-tgl ~ .menu .menu-btn {
	position: absolute;
	box-sizing: border-box;
	right: -120px;
	width: 100px;
	top: 20px;
	font-family: Verdana;
	cursor: pointer;
	background-color: #101925;
	color: #fff;
	padding: 10px 18px;
	border-radius: 5px;
	text-align: center;
}
/* Aqui um bônus, para mudar o texto dentro do nosso label
   nós utilizaremos o pseudo-elemento ::after, assim 
   temos algo ainda mais interativo pro usuário,
   quando estiver fechado irá aparecer "ABRIR" */
#menu-tgl ~ .menu .menu-btn::after {
	content:"ABRIR";
}

/* Aqui vem a magia do aparecimento da barra
   quando nosso input#menu-tgl estiver selecionado 
   a nossa barra irá deslizar suavemente para fora 
   da lateral esquerda */
#menu-tgl:checked ~ .menu {
	transform: translateX(0%);
}
/* e o texto será alterado para "FECHAR" */
#menu-tgl:checked ~ .menu .menu-btn::after {
	content:"fechar";
}
</code></pre>
</div>

<h3>Conclusão</h3>
<p>Isso aí galera, feito tudo isso o resultado deve ser parecido com esse abaixo, tirando fato de eu ter adicionado um pouco de estilo na nossa lista.</p>
<p data-height="265" data-theme-id="dark" data-slug-hash="XpMXpR" data-default-tab="result" data-user="mathmesquita" data-embed-version="2" data-pen-title="Menu toggle sem JS" class="codepen">See the Pen <a href="http://codepen.io/mathmesquita/pen/XpMXpR/">Menu toggle sem JS</a> by Matheus Mesquita (<a href="http://codepen.io/mathmesquita">@mathmesquita</a>) on <a href="http://codepen.io">CodePen</a>.</p>
<script async src="https://production-assets.codepen.io/assets/embed/ei.js"></script>
<p>Espero que tenham gostado desse post, ficou curtinho mas o importante é pegar o feeling desses seletores/pseudo-classes/elementos para fazer efeitos legais em seus sites sem necessidade de trabalho extra na parte de scripting.</p>
<p>Se voce não entendeu bulhufas sobre o transition e o transform que aplicamos no nosso elemento, terça-feira pode colar aqui no blog que irei fazer um post sobre isso muito legal !(e alterar essa frase aqui para botar o link do post).</p>
<p>Muito obrigado pelo seu tempo lendo, e estou sempre com o f5 no post esperando comentários, sugestões e criticas ! Abraços e até a próxima ! </p>

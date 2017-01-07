---
layout: post
title:  "Bem-vindo ao Canvas #1"
image: "assets/bem-vindo-canvas/cover.png"
date:   2016-01-07 08:00:00 -0200
color_template: "canvas"
tags: html5 canvas javascript
resume: "Acha muito legal aqueles efeitos de particulas nos sites mas nunca entendeu muito bem como são feitos? Também sempre tive essa dúvida, até descobrir o canvas."
topic: canvas
comments: true
---

<h3>Introdução</h3>
<p>Oi galera, esse é meu primero post, depois de 2 meses pronto o blog, <i>inclusive nesse dominio</i>, finalmente consegui tempo e um bom tópico para dar início a essa nova fase nesse novo ano da minha vida. Em breve farei um post falando mais sobre mim, mas tem um pequeno resumo na área de <a href="/sobre" target="_blank">sobre</a> caso voce tenha interesse. Sem mais, vamos ao que interessa.</p>
<p>Nesse post trataremos do básico sobre o canvas, que são as formas, mas pretendo fazer uma série de 3 posts(1/semana) finalizando com um efeito de particulas maneirissimo, utilizando eventos de mouse, aleatoriedade, fisica... Será bem legal !</p>

<h3>O que é canvas?</h3>
<p>Antes de mexer com ele, seria útil entendermos o que é ele.</p>
<p>O <i>canvas</i> veio com o HTML5 e a proposta era bem simples, apresentar computação gráfica na web(no lugar do flash). Ele pode ser utilizado para apresentação de elementos em 2D e 3D com o WebGL, com o <i>canvas</i> poderemos apresentar desenhos estáticos ou animados de forma simples ou complexa.</p>
<p>O <i>canvas</i> em si é somente uma tag html, para realizar os desenhos e as animações dentro da tag, ou palco, precisaremos utilizar o <b>javascript</b>, ajuda se tivermos um pouco de conhecimento sobre formas como retangulos, circulos e linhas.</p>

<h3>Iniciando</h3>
<p>Antes de tudo, precisamos criar nosso arquivo html e js, no html precisaremos somente adicionar a tag &lt;canvas&gt;&lt;/canvas&gt;.</p>
<div class="code html">
	<span class="file-name">index.html</span>
	<pre><code>
...
&lt;canvas id="mycanvas" width="150" height="150"&gt;&lt;/canvas&gt;
&lt;!-- 
   o ID não é obrigatório, só vai facilitar nosso trabalho mais na frente 
   Adicionar o width e height direto na tag é indicado, caso voce
   faça por CSS seus desenhos no palco ficarão distorcidos
   width e height padrão é 300x150 pixels respectivamente
--&gt;
...
	</code></pre>
</div>
<p>No nosso arquivo javascript, iremos fazer uma função simples chamada draw e avisar ao navegador que é para ela ser executada quando a página for carregada.</p>
<div class="code javascript">
	<span class="file-name">index.js</span>
	<pre><code>
window.onload = draw;

function draw(){
   // aqui obtemos a referencia ao DOMNode do canvas,
   // por isso utilizamos um id acima, para faciliar essa obtenção
   // voce também poderia pegar pelo nome da tag ou class
   var canvas = document.getElementById("mycanvas");

   // aqui informamos ao navegador que estaremos 
   // utilizando o contexto bidimensional/2D
   var context = canvas.getContext("2d");
}
	</code></pre>
</div>
<p>Criados esses dois arquivos, vamos as formas.</p>

<h3>Formas</h3>
<p>Nosso palco 2D segue um sistema de coordenadas cartesianas com a origem começando em (0, 0) no topo superior esquerdo. X incrementa para a direita, e Y incrementa para baixo, no próximo post iremos ver como "simular" um plano cartesiano tradicional com a origem começando no canto inferior esquerdo.</p>
<p>Nas formas, existem somente 2 primitivos, o retângulo e o path("caminho") que nada mais é que um conjunto de pontos que concretizam uma forma.</p>

<h4>Retângulos</h4>
<p>O retângulo é uma forma primitiva e a mais comum, com essa forma poderemos criar backgrounds, limpar nosso palco e desenhar coisas também. Para criar nosso retângulo utilizaremos a função <a href="https://developer.mozilla.org/en-US/docs/Web/API/CanvasRenderingContext2D/fillRect">.fillRect(x, y, width, height)</a> .</p>
<div class="code javascript">
	<span class="file-name">index.js</span>
	<pre><code>
window.onload = draw;

function draw(){
   var canvas = document.getElementById("mycanvas");
   var context = canvas.getContext("2d");

   // em nosso contexto, criaremos nosso retângulo
   context.fillRect(0, 0, 150, 150);
   // deverá aparecer um retângulo preto tapando
   // toda nossa área do canvas, como se fosse
   // um background
}
	</code></pre>
</div>

<h4>Círculos</h4>
<p>Os <a href="https://pt.wikipedia.org/wiki/C%C3%ADrculo">círculos</a> são <i>caminhos</i> formados por um conjuntos de pontos que seguem uma relação trigonométrica. As extremidades sempre serão equidistantes do centro, baseado em um raio que iremos determinar.</p>
<p>Para não termos que desenhar ponto a ponto, a canvas API nos fornece uma função de utilidade que irá criar esse círculo para nós, <a href="https://developer.mozilla.org/en-US/docs/Web/API/CanvasRenderingContext2D/arc">.arc(x, y, raio, anguloInicial, anguloFinal [, sentidoAntiHorario])</a> .</p>
<div class="code javascript">
	<span class="file-name">index.js</span>
	<pre><code>
window.onload = draw;

function draw(){
   var canvas = document.getElementById("mycanvas");
   var context = canvas.getContext("2d");

   context.fillRect(0, 0, 150, 150);

   // existem passos muito importantes no processo
   // de desenhar um caminho, são eles:
   // 1º iniciar um caminho
   context.beginPath();

   // 2º desenhar o caminho que desejamos
   context.arc(75, 75, 40, 0, Math.PI*2);

   // 3º finalizar o caminho
   context.closePath();

   // 4º adicinar um preenchimento ou borda 
   //    na forma que criamos
   context.strokeStyle = "white";
   context.stroke();
}
	</code></pre>
</div>
<p>Se voce seguiu os passos corretamente até aqui, surgiu um círculo no melhor estilo "O chamado" no meio do seu palco.</p>

<h4>Linhas</h4>
<p>Linhas são caminhos formados por uma função onde y = ax + b, ou famosa equação da reta. Novamente a API de "caminhos" nos fornece uma função de utilizade que ja desenha a linha inteiramente para nós, <a href="https://developer.mozilla.org/en-US/docs/Web/API/CanvasRenderingContext2D/lineTo">.lineTo(x, y)</a> .</p>
<div class="code javascript">
	<span class="file-name">index.js</span>
	<pre><code>
window.onload = draw;

function draw(){
   var canvas = document.getElementById("mycanvas");
   var context = canvas.getContext("2d");

   context.fillRect(0, 0, 150, 150);

   context.beginPath();
   context.arc(75, 75, 30, 0, Math.PI*2);
   context.closePath();
   context.strokeStyle = "white";
   context.stroke();

   // iremos seguir todos os passos de caminho
   // abrir, desenhar, fechar, e adcionar borda/preenchimento
   context.beginPath();

   // a função .moveTo(x, y) serve para mudar
   // o local em que o caminho está posicionado
   // como se fosse voce levantando o lapís de 
   // uma folha, e encostando em outro local
   context.moveTo(75, 17);

   // nosso triângulo
   context.lineTo(135, 107);
   context.lineTo(15, 107);
   context.lineTo(75, 17);
   context.lineTo(75, 107);

   // outra forma de criar um retângulo
   // só que em forma de "caminho", não primitivo
   context.rect(5, 5, 140, 140);

   // fechar e fazer o contorno do triangulo
   context.closePath();
   context.stroke();
}
	</code></pre>
</div>
<p>Se voce fez todos os passos corretamente, surgiu uma figura que voce talvez conheça. É o símbolo das relíquias da morte em Harry Potter ! :D</p>
<p data-height="265" data-theme-id="dark" data-slug-hash="oBXGwm" data-default-tab="result" data-user="mathmesquita" data-embed-version="2" data-pen-title="Reliquias mortais" class="codepen">See the Pen <a href="http://codepen.io/mathmesquita/pen/oBXGwm/">Reliquias mortais</a> by Matheus Mesquita (<a href="http://codepen.io/mathmesquita">@mathmesquita</a>) on <a href="http://codepen.io">CodePen</a>.</p>
<script async src="https://production-assets.codepen.io/assets/embed/ei.js"></script>

<h3>Conclusão</h3>
<p>Esse post ficou maior do que o esperado, quando comecei a escrever não achei que ficaria tão grande assim. Não conseguimos abordar todas as formas. Mas não se preocupem, que essas são as principais.</p>
<p>No próximo post iremos abordar transformações matriciais, (translação, rotação e escala), apesar do nome dificil, tudo isso é bem comum no CSS3, e aqui vai funcionar de forma semelhante.</p>
<p>Espero de coração que voces tenham gostado desse post, foi o meu primeiro e estou sempre aberto a críticas e sugestões nos comentários ou no meu <a href="mailto:{{ site.email }}">email</a>!</p>
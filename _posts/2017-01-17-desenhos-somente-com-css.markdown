---
title: "Desenhos utilizando somente CSS"
layout: post
image: "/assets/desenhos-com-css/cover.jpg"
date:   2017-01-17 08:00:00 -0200
color_template: "css"
tags: css3 inspiracao
resume: "Ja abriu o codepen e viu um monte de desenhos feitos no CSS? Mas você não tem idéia nem de como fazer um triângulo ? Esse post é pra você. 8D"
topic: css
comments: true
---

<h3>Introdução</h3>
<p>Fala galera, tudo bem com vocês? o post de hoje vai falar sobre as mais diversas formas que conseguimos fazer no CSS, como triangulos, circulos, losangos, e foi feito para aquelas pessoas que quando se deparam com uma seta, ou uma dobrinha no layout ja entram em desespero achando que vão precisar recortar mais uma imagem pra ser carregada no site. </p>
<p>Sei que tem uma série em aberto <a href="https://mathmesquita.me/2016/01/07/bem-vindo-ao-canvas">sobre canvas</a>, e nele também conseguimos fazer várias formas geométricas, mas o negócio é o seguinte, nem sempre precisamos iniciar uma ferramenta tão poderosa quanto o canvas para fazer desenhos tão simples.</p>
<p>E como de costume, no final teremos um resultado bem legal para termos uma idéia daquilo que podemos fazer utilizando somente html + css.</p>

<h3>Formas</h3>
<p>Antes de começarmos a por a mão na massa para fazer nosso desenho, vamos falar de como fazer as formas usando o CSS.</p>
<p><span class="important">obs:</span>para ficar simples nas áreas de código aqui do blog, irei implementar somente o CSS e deixarei como deve ficar a tag html nos comentarios.</p>

<h4>Retângulos</h4>
<p>Retângulos são a forma mais simples(sempre), e você praticamente monta retângulos aos montes com o CSS, afinal todo container novo ja vem nesse formato quadradão.</p>
<p>O máximo que vamos fazer aqui é definir uma altura/largura e um background-color para utilizarmos essa forma onde quer que seja necessária no nosso desenho.</p>
<div class="code css">
	<span class="file-name">index.css</span>
	<pre><code class="css">
...
#retangulo1{
	width: 100px;
	/* quadrados são retangulos com width = height  */
	height: 50px;
	background-color: #145267;
}

/* no html: &lt;div id="retangulo1" &gt;&lt;/div&gt;
...
</code></pre>
</div>
<p data-height="190" data-theme-id="dark" data-slug-hash="oBYaaL" data-default-tab="css,result" data-user="mathmesquita" data-embed-version="2" data-pen-title="oBYaaL" class="codepen">See the Pen <a href="http://codepen.io/mathmesquita/pen/oBYaaL/">oBYaaL</a> by Matheus Mesquita (<a href="http://codepen.io/mathmesquita">@mathmesquita</a>) on <a href="http://codepen.io">CodePen</a>.</p>
<script async src="https://production-assets.codepen.io/assets/embed/ei.js"></script>

<h4>Circulos</h4>
<p>Talvez antigamente fazer um circulo fosse uma tarefa muito difícil, mas hoje em dia graças a propriedade border-radius, nós conseguimos fazer eles com tranquilidade.</p>
<p>Se adicionarmos os pseudo-elementos ::after e ::before, conseguimos até fazer segmentos do circulo.</p>
<div class="code css">
	<span class="file-name">index.css</span>
	<pre><code class="css">
...
#circulo1{
	/* para o circulo ser perfeito, ele tem que ser um quadrado
	   caso contrario acabariamos formando uma elipse */
	width: 100px;
	height: 100px;
	border-radius: 50%;
	background-color: #145267;
}

#circulo1quarto{
	width: 100px;
	height: 100px;
	border-top-left-radius: 100%;
	background-color: #145267;	
}

/* no html: &lt;div id="circulo1" &gt;&lt;/div&gt;
   no html: &lt;div id="circulo1quarto" &gt;&lt;/div&gt;
...	
</code></pre>
</div>
<p data-height="298" data-theme-id="dark" data-slug-hash="apBRQO" data-default-tab="css,result" data-user="mathmesquita" data-embed-version="2" data-pen-title="apBRQO" class="codepen">See the Pen <a href="http://codepen.io/mathmesquita/pen/apBRQO/">apBRQO</a> by Matheus Mesquita (<a href="http://codepen.io/mathmesquita">@mathmesquita</a>) on <a href="http://codepen.io">CodePen</a>.</p>

<h3>Triangulos</h3>
<p>Acho que é uma das formas mais legais de se fazer no CSS, a principio pode ser esquisito, mas iremos faze-la utilizando somente o border-width e border-color.</p>
<div class="code css">
	<span class="file-name">index.css</span>
	<pre><code class="css">
...
#triangulo1{
	/*  para a menos que usemos a propriedade box-sizing:border-box
	    nós precisamos definir o width com o valor 0 */
	width: 0px;

	/* a borda que terá cor dependerá de qual sentido
	   você quer seu triangulo apontando */
	border-left: 15px solid transparent;
	border-right: 15px solid transparent;
	/* o tamanho da base do nosso triangulo é definida 
	   pela soma das duas bordas transparentes 
	   15 + 15 = 30px de base que teremos */

	/* como queremos nosso triangulo apontando para cima
	   botamos a borda em baixo */
	border-bottom: 30px solid #145267;
	/* o tamanho da borda colorida irá definir a altura
	   do nosso triangulo, 30px de altura que teremos */

	/* para saber a borda correta é que receberá a cor
	   é só botar na borda oposta a o lado que você apontará
	   Ex: para esquerda < borda na direita */
}

/* no html: &lt;div id="triangulo1" &gt;&lt;/div&gt;
...	
</code></pre>
</div>
<p data-height="186" data-theme-id="dark" data-slug-hash="EZNdGa" data-default-tab="css,result" data-user="mathmesquita" data-embed-version="2" data-pen-title="EZNdGa" class="codepen">See the Pen <a href="http://codepen.io/mathmesquita/pen/EZNdGa/">EZNdGa</a> by Matheus Mesquita (<a href="http://codepen.io/mathmesquita">@mathmesquita</a>) on <a href="http://codepen.io">CodePen</a>.</p>

<h3>Losangos</h3>
<p>Apesar de não ser uma forma primitiva, decidi dedicar esse espaço para falar dos losangos mais como uma forma de apresentar para vocês o que conseguimos fazer com as outras formas básicas.</p>
<div class="code css">
	<span class="file-name">index.css</span>
	<pre><code class="css">
...
#losango{
	/* relative para nossos elementos não fugirem */
	position:relative;
	/* escolhemos o tamanho do nosso losango */
	width: 50px;
	height: 140px;
}
#losango::after{
	/* content é OBRIGATÓRIO, se não tiver, o pseudo-elemento
	   não será renderizado no DOM. */
	content:'';
	position:absolute;
	/* importante, a direção de alinhamento(bottom, top, right, left)
	   tem que ser declarada no mesmo sentido que a nossa borda "colorida"
	   o valor 70 é a metade da altura do losango, se o losango
	   estivesse deitado, seria metade da largura, 
	   e alinhado ao left+border-left "colorida" */
	bottom: 70px;
	border-bottom: 70px solid #192314;
	border-left: 25px solid transparent;
	border-right:25px solid transparent;
}
#losango::before{
	content:'';
	position: absolute;
	/* alinha ao top, borda no top "colorida" */
	top: 70px;
	border-top: 70px solid #192314;
	border-left: 25px solid transparent;
	border-right:25px solid transparent;
}

/* no html: &lt;div id="losango" &gt;&lt;/div&gt;
...	
</code></pre>
</div>
<p data-height="265" data-theme-id="dark" data-slug-hash="VPmENp" data-default-tab="css,result" data-user="mathmesquita" data-embed-version="2" data-pen-title="VPmENp" class="codepen">See the Pen <a href="http://codepen.io/mathmesquita/pen/VPmENp/">VPmENp</a> by Matheus Mesquita (<a href="http://codepen.io/mathmesquita">@mathmesquita</a>) on <a href="http://codepen.io">CodePen</a>.</p>

<h3>Só isso ?</h3>
<p><b>"Show matheus, você ensinou como fazer as formas, mas eu não tenho idéia de como posso aplicar isso".</b></p>
<p>Se você pensou isso, eu preparei esse exemplo abaixo para te mostrar um desenho de uma casa simples e que ja da pra fazer com todo o conhecimento adquirido nesse post.</p>
<p data-height="365" data-theme-id="dark" data-slug-hash="VPmGLV" data-default-tab="result" data-user="mathmesquita" data-embed-version="2" data-pen-title="Casinha desenhada com CSS" class="codepen">See the Pen <a href="http://codepen.io/mathmesquita/pen/VPmGLV/">Casinha desenhada com CSS</a> by Matheus Mesquita (<a href="http://codepen.io/mathmesquita">@mathmesquita</a>) on <a href="http://codepen.io">CodePen</a>.</p>

<h3>Dificil de impressionar</h3>
<p>Se você é desses que é dificil de impressionar e não achou nada demais naquela casinha simples, segue aqui um outro exemplo do que podemos fazer <b>usando somente CSS</b>.</p>
<p data-height="605" data-theme-id="dark" data-slug-hash="mRraOd" data-default-tab="result" data-user="renduh" data-embed-version="2" data-pen-title="CSS ONLY Girl Running" class="codepen">See the Pen <a href="http://codepen.io/renduh/pen/mRraOd/">CSS ONLY Girl Running</a> by Adam Walker (<a href="http://codepen.io/renduh">@renduh</a>) on <a href="http://codepen.io">CodePen</a>.</p>
<p>Posso fazer um post explicando como montar um desenho semelhante a esse, lembrando que ele não foi feito por mim! E sim por <a href="http://codepen.io/renduh/">esse usuario aqui</a>.</p>

<h3>Conclusão</h3>
<p>Esse post foi bem básico, falamos somente de formas simples e no final dei um exemplo do que ja é possivel montar com o conhecimento adquirido aqui.</p>
<p>Lembrando que infelizmente não existe fórmula para fazer um bom desenho, o negocio é ir fazendo aos poucos e ajustando da forma que ficar legal, praticando bastante voce jaja irá conseguir fazer algo parecido com aquela garotinha pulando.</p>
<p>Por fim espero que tenham gostado, e principalmente espero seus comentários, pedidos, criticas e sugestões, além claro, de dicas caso tenha alguma! :D Muito obrigado pelo seu tempo, se você leu até aqui, e até o próximo post õ7</p>
<p>Me avisem se vocês querem um segundo post sobre esse tema, mostrando como fazer desenhos mais complexos ! :D</p>

---
layout: post
title: "Bem-vindo ao Canvas #2"
image: "/assets/bem-vindo-canvas/cover2.jpg"
date:   2017-01-14 08:00:00 -0200
color_template: "canvas"
tags: html5 canvas javascript
resume: "Não podemos nos ater somente as formas, existem muitas coisas que podemos fazer no nosso palco. E hoje iremos falar das transformações. ;)"
topic: canvas
comments: true
---

<h3>Introdução</h3>
<p>Fala pessoal, venho informar que é um prazer estar com vocês novamente, e que esse é o segundo post da série "Bem-vindo ao canvas", pra quem não viu o primeiro é só vir <a href="https://mathmesquita.me/2016/01/07/bem-vindo-ao-canvas.html">aqui</a>. Lembrando que a intenção dessa série em 3 posts é nos introduzir aos princípios do canvas, e no final fazer um efeito de particulas muito maneiro.</p>
<p>Nesse post iremos falar sobre as principais transformações matriciais(translação, rotação e escala). No final novamente irá surgir algum desenho interesssante(eu acho), sem mais, vamos ao que interessa.</p>
<p>Se voce não faz idéia do que é o canvas, no <a href="https://mathmesquita.me/2016/01/07/bem-vindo-ao-canvas.html">primeiro post</a> tem uma explicação bem resumida dessa ótima ferramenta do HTML5.</p>

<h3>Antes de tudo</h3>
<p>Precismos da nossa tag &lt;canvas&gt;&lt;/canvas&gt; e inicializar nosso contexto</p>
<div class="code html">
	<span class="file-name">index.html</span>
	<pre><code>
&lt;style&gt;
   *{padding:0;margin:0;}
   // esse position é só ter uma barra de rolagem
   // overflow: hidden no body tb é blz XD
   #mycanvas{position:fixed;}
&lt;/style&gt;
...
&lt;canvas id="mycanvas" width="150" height="150"&gt;&lt;/canvas&gt;
...
	</code></pre>
</div>
<div class="code javascript">
	<span class="file-name">index.js</span>
	<pre><code>
window.onload = draw;

function draw(){
   // Obtendo nosso nó de referencia
   var canvas = document.getElementById("mycanvas");
   // ajustar as dimensões para ocupar a página inteira
   canvas.width = window.innerWidth;
   canvas.height = window.innerHeight;
   
   // inicializando o contexto em 2 dimensões
   var context = canvas.getContext("2d");
}
	</code></pre>
</div>

<h3>Continuamos usando as formas</h3>
<p>Infelizmente, aprender algo novo não cancela algo antigo, pelo menos não nesse caso, aqui precisaremos usar os "caminhos"(paths), para demonstrar o que iremos fazer.</p>
<p>Como o desenho desse post ficará um pouco mais complexo, vamos abraçar toda a nossa peça em um objeto.</p>
<p>Mãos a obra, vamos fazer nosso desenho.</p>
<div class="code javascript">
   <span class="file-name">index.js</span>
   <pre><code>
window.onload = draw;

function draw(){
   var canvas = document.getElementById("mycanvas");
   canvas.width = window.innerWidth;
   canvas.height = window.innerHeight;

   // retiramos o contexto dessa função
   // e transferimos para o objeto sky
   sky.paint(canvas);
}

var sky = {
   canvas:null,
   ctx:null,
   paint:function(canvas){
      // setar as configurações iniciais
      this.canvas = canvas;
      // atribuindo o contexto a uma variavel
      // no nosso objeto
      this.ctx = this.canvas.getContext("2d");

      // vamos simplificar nosso código e usar
      // funções especificas para cada parte
      // do nosso desenho
      this.drawSky();
   },
   drawSky:function(){
      // função responsável por desenhar nosso céu
      // (oh really?)
      this.ctx.fillStyle = "#0c1644"; // fillStyle para escolher a cor
                                      // de preenchimento do nosso fundo
      
      // aqui o céu é desenhado para ocupar
      // o palco inteiro
      this.ctx.fillRect(0, 0, this.canvas.width, this.canvas.height);
   }
}
   </code></pre>
</div>
<p>Até ai show, nada de novo a não ser o <a href="https://developer.mozilla.org/en-US/docs/Web/API/CanvasRenderingContext2D/fillStyle">fillStyle</a>, que não é nada tão complicado de entender, ele é semelhante ao <a href="https://developer.mozilla.org/en-US/docs/Web/API/CanvasRenderingContext2D/strokeStyle">strokeStyle</a>, mas vai cuidar da cor do preenchimento das nossas formas.</p>

<h3>Transformações</h3>
<p>Consegui pensar em um exemplo muito legal, nós iremos em uma função, pra desenhar uma unica forma utilizar 3 das 4 transformações possíveis, rotação, translação e escala.</p>
<p>Infelizmente não da pra pular a parte teórica chatona, <strike>não para mim</strike>, então pra não confundir tudo, irei primeiro falar sobre cada transformação separadamente, e por fim vamos ao exemplo utilizando as 3 </p>

<h4>Translação</h4>
<img src="https://www.w3.org/TR/2012/WD-css3-transforms-20120403/examples/svg-translate.png" alt="">
<p>A transformação de <b>translação</b>, é realizada quando voce quer mudar a posição de um objeto, <b>demonstrado acima</b>.</p>
<p>No nosso caso do canvas, nós não conseguimos criar objetos inteiros e move-los de forma sólida para outro ponto, dessa maneira, a transformação de <b>translação</b> no canvas serve para mudar a origem do nosso palco, <b>demonstrado abaixo</b>, e a partir dali iremos desenhar a forma que planejavamos translocar.</p>
<img src="https://mdn.mozillademos.org/files/234/Canvas_grid_translate.png" alt="">
<p>A função responsavel por isso no canvas é a <a href="https://developer.mozilla.org/en-US/docs/Web/API/CanvasRenderingContext2D/translate">.translate(x, y)</a>.</p>

<h4>Escala</h4>
<img src="http://orm-chimera-prod.s3.amazonaws.com/1234000001814/figs/web/i3dp_0207.png" alt="">
<p>Como demonstrado na figura acima, a transformação de <b>escala</b>, é nada mais do que aumentar algum eixo proporcionalmente ao valor passado, <b>lembrando que a escala inicial/padrão é sempre (1, 1)</b>, sendo assim na imagem vemos que a figura foi diminuida pela metade(ou 1/2 = 0.5), se quisesse dobrar o tamanho do desenho ele teria usado (2, 2).</p>
<p>A função que iremos utilizar será <a href="https://developer.mozilla.org/en-US/docs/Web/API/CanvasRenderingContext2D/scale">.scale(x, y)</a>.</p>
<p><span class="important">Fato interessante:</span> nós podemos espelhar nosso canvas utilizando uma transformação negativa (-1, 1), (1, -1) ou (-1, -1)</p>
<div class="code javascript">
   <span class="file-name">espelhado.js</span>
   <pre><code>
...
   // caso apliquemos
   context.scale(1, -1);
   // nosso sistema de coordenadas ficará
   // com a origem em baixo esquerda, como
   // um tradicional plano cartersiano :D
...
</code></pre>
</div>

<h4>Rotação</h4>
<img src="http://i612.photobucket.com/albums/tt209/fredrikmork/rotatetransform.png" alt="">
<p><b>Rotação</b> a última das transformações mais utilizadas, ela também pode ser considerada a mais simples, responsável apenas por rotacionar um objeto no sentido horário.</p>
<p>Novamente, <b>no canvas não controlamos as formas</b>, só controlamos o palco, sendo assim o que acontece é que o nosso palco será rotacionado no sentido horário como demonstrado abaixo.</p>
<img src="https://mdn.mozillademos.org/files/233/Canvas_grid_rotate.png" alt="">
<p>Utilizaremos <a href="https://developer.mozilla.org/en-US/docs/Web/API/CanvasRenderingContext2D/rotate">.rotate(angulo)</a> para aplicar essa transformação.</p>

<h4>muito importante</h4>
<p>Todas as transformações que fazemos no nosso contexto ficam salvas, então sempre é necessão chamar um <a href="https://developer.mozilla.org/en-US/docs/Web/API/CanvasRenderingContext2D/save">.save()</a> e <a href="https://developer.mozilla.org/en-US/docs/Web/API/CanvasRenderingContext2D/restore">.restore()</a>, antes e depois de aplicar as transformações.</p>
<div class="code javascript">
   <span class="file-name">indicado_fazer.js</span>
   <pre><code>
...
   context.save(); // salvo o estado atual
   // faz as transformações
   context.restore(); // retorna ao estado salvo
...
</code></pre>
</div>

<h4>Exemplo</h4>
<p>Visto isso, vamos à continuação do nosso desenho.</p>
<div class="code javascript">
   <span class="file-name">index.js</span>
   <pre><code>
window.onload = draw;

function draw(){ ... } // o código é o mesmo de lá de cima
                       // só omiti ele para não ficar
                       // ocupando mt espaço na área de code

var sky = {
   canvas:null,
   ctx:null,
   paint:function(canvas){
      this.canvas = canvas;
      this.ctx = this.canvas.getContext("2d");

      this.drawSky();

      // manda desenhar a estrela do nosso céu
      this.drawStar();
   },
   drawSky:function(){ ... },
   drawStar:function(){
      // primeira coisa sempre é salvar o estado atual
      this.ctx.save();

      // nossa primeira transformação, mudamos a origem
      // para 100px distante em x e y
      this.ctx.translate(100, 100);

      // definindo o tamanho da nossa estrela
      // aqui definimos qual será a escala da nossa estrela 
      // de 0 até 1.5
      var escala = Math.random()*1.5;
      // nossa segunda transformação, alteramos o tamanho
      // da escala para o valor da variavel calculada acima
      this.ctx.scale(escala, escala);

      // aqui definimos a distancia das pontas das nossas estrelas
      var raioMax = 20;
      // e aqui a distancia minima das nossas pontas
      var raioMin = 8;

      // iniciamos nosso caminho
      this.ctx.beginPath();

      // aplicamos uma rotação aleatória para a
      // estrela ficar um pouco inclinada
      this.ctx.rotate(Math.random());

      // movemos nossa ponta da caneta para a ponta mais distance
      this.ctx.moveTo(raioMax, 0);
      
      // e aqui vem o for do desenho que fica fazendo uma linha
      // para ponta distante e a interna
      for(var i = 0; i < 10; i++){
         // a cada loop nós adicionamos pi/5 a rotação anterior
         // isso ocorre porque não chamamos o método .restore()
         // se tivessemos chamado ele toda vez fariamos uma
         // linha na mesma direção.
         this.ctx.rotate(Math.PI/5);
      
         // condicional simples para verificar se é para desenhar
         // para a ponta externa ou para a interna
         if(i%2 == 0){
            this.ctx.lineTo(raioMin, 0)
         }else{
            this.ctx.lineTo(raioMax, 0)
         }
      }
      
      // fechar o caminho, MUITO IMPORTANTE
      this.ctx.closePath();
      
      // escolher a cor do preenchimento da nossa estrela
      this.ctx.fillStyle = "white";
      // preencher
      this.ctx.fill();
      
      // e retornar ao estado anterior, assim não atrapalharemos
      // nenhum desenho feito por outra função
      this.ctx.restore();
   }
}
</code></pre>
</div>

<h3>Deixando interessante</h3>
<p>Na introdução do post eu prometi que surgiria alguma coisa legal, até o momento só tem uma estrela no meio de um troço azul.</p>
<p>Pra deixar interessante vamos direto ao código, bora encher esse céu de estrelas.</p>
<div class="code javascript">
   <span class="file-name">index.js</span>
   <pre><code>
window.onload = draw;

function draw(){ ... } 

var sky = {
   quantStars:70, // aqui setamos quantas estrelas queremos
                  // no nosso céu
   canvas:null,
   ctx:null,
   paint:function(canvas){
      this.canvas = canvas;
      this.ctx = this.canvas.getContext("2d");

      this.drawSky();

      this.populateSky(); // vamos popular o céu com estrelas !
   },
   drawSky:function(){ ... },
   populateSky:function(){
      // um loop simples que cuida de criar as
      // nossas estrelas
      for(var i=0; i < this.quantStars; i++;){
         // a estrela vai ser gerada em algum lugar aleatório 
         // sem ultrapassar o tamanho da tela
         this.drawStar(Math.random()*this.canvas.width,
                       Math.random()*this.canvas.height);
      }
   },
   drawStar:function(posX, posY){ // só fizemos essa alteração aqui
      this.ctx.save();
      
      // pra poder criar a estrela no local que passarmos como parametro
      this.ctx.translate(posX, posY);

      var escala = Math.random()*1.5;
      this.ctx.scale(escala, escala);

      var raioMax = 20;
      var raioMin = 8;

      this.ctx.beginPath();
      this.ctx.rotate(Math.random());
      this.ctx.moveTo(raioMax, 0);
      for(var i = 0; i < 10; i++){
         this.ctx.rotate(Math.PI/5);
         if(i%2 == 0){
            this.ctx.lineTo(raioMin, 0)
         }else{
            this.ctx.lineTo(raioMax, 0)
         }
      }
      
      this.ctx.closePath();

      this.ctx.fillStyle = "white";
      this.ctx.fill();
      
      this.ctx.restore();
   }
}
</code></pre>
</div>
<p>Seguidos todos os passos corretamente, voce provavelmente teve um resultado idêntico a esse !(só ta mudando toda vez que voce recarrega a página :p)</p>
<p data-height="339" data-theme-id="dark" data-slug-hash="LxZqaz" data-default-tab="result" data-user="mathmesquita" data-embed-version="2" data-pen-title="Céu estrelado" class="codepen">See the Pen <a href="http://codepen.io/mathmesquita/pen/LxZqaz/">Céu estrelado</a> by Matheus Mesquita (<a href="http://codepen.io/mathmesquita">@mathmesquita</a>) on <a href="http://codepen.io">CodePen</a>.</p>
<script async src="https://production-assets.codepen.io/assets/embed/ei.js"></script>

<h3>Conclusão</h3>
<p>Wow, esse post acabou conseguindo ser maior que o outro!! :O Mas é isso aí galera, conforme vai aumentando a complexidade os posts vão ficando maiores, esse talvez foi mais divertido que o outro pois teve um resultado que ja começa a traçar uma linha para o que queremos de particulas(<strike>se voce mudar a estrela pra um circulo voce acabou de gerar as particulas</strike>).</p>
<p>A série continua semana que vem(chegando ao seu final :[ ), mas relaxe que ja estou planejando mais coisas para brincar com o canvas !! Quem sabe uma série de canvas avançado ? ou fazendo um jogo na unha ? Enfim !!! muitas idéias e por hoje paramos aqui. </p>
<p>Qualquer dúvida, sugestão, elogio, xingamento convite para uma conversa, estarei nos comentários !! Espero que tenham gostado e até a próxima. õ7</p>
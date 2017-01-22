---
layout: post
title: "Bem-vindo ao Canvas #3"
image: "/assets/bem-vindo-canvas/cover3.jpg"
date:   2017-01-21 08:00:00 -0200
color_template: "canvas"
tags: html5 canvas javascript
resume: "Episódio final dessa saga pela busca do efeito de partículas. SPOILER: finalmente nosso efeito de particulas é criado !"
topic: canvas
comments: true
---

<h3>Introdução</h3>
<p>Fala galera, olha eu aqui de novo no último post dessa série que vai abrir seus horizontes para várias coisas que podemos adicionar nos sites que desenvolvemos.</p>
<p>Se voce não viu nem o <a target="_blank" href="/2016/01/07/bem-vindo-ao-canvas">primeiro</a> nem o <a target="_blank" href="/2017/01/07/bem-vindo-ao-canvas">segundo</a> post dessa série, indico fortemente voce deixar esse post nessa aba e ir dar uma lida neles, <b>se não o que vai acontecer aqui pode ficar muito confuso</b>.</p>
<p>Nesse post iremos utilizar todo o conhecimento obtido nos outros posts e fazer um efeito que ja vai estar <i>ready to implement</i> no seu site. :)</p>

<h3>Aquele início de sempre</h3>
<p>Se voces não lembravam onde tudo começa, a primeira coisa que devemos definir é a nossa tag canvas e os estilos para deixar nosso canvas ocupando a tela inteira. Dessa vez irei deixar o background da página sendo o fundo do nosso canvas e nele iremos desenhar somente as partículas !</p>

<div class="code html">
	<span class="file-name">index.html</span>
	<pre><code>
&lt;style&gt;
   *{padding:0;margin:0;}
   #mycanvas{position:fixed;}
   body{background-color: #0a0014;}
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
   var canvas = document.getElementById("mycanvas");
   // IMPORTANTE !
   // lembrem que o nosso canvas não aceita width/height 100% via css
   // sendo assim temos que definir pelo JS
   canvas.width = window.innerWidth;
   canvas.height = window.innerHeight;
   
   var context = canvas.getContext("2d");
}
	</code></pre>
</div>

<h3>As partículas</h3>
<p>A primeira coisa a se fazer é a programação das nossas particulas, definir aceleração, as funções de desenho, movimento etc.</p>
<div class="code javascript">
	<span class="file-name">index.js</span>
	<pre><code>
window.onload = draw;
function draw(){ ... }

/* criamos o nosso function constructor,
   passando como parametros:
   ctx = context do nosso elemento canvas
   x = posição inicial X da particula
   y = posição inicial Y da particula
   vX = aceleração das particulas em X
   vY = aceleração das particulas em Y */
function Particle(ctx, x, y, vX, vY){
   // dentro do FC nós declaramos o tamanho 
   // default da nossa particula como 1
   this.radius = 1;
   
   // e adicionamos todos os argumentos
   // no nosso objeto particula
   this.ctx = ctx;
   this.x = x;
   this.y = y;
   this.vX = vX;
   this.vY = vY;

   // por fim desenhamos ela a primeira vez na tela, na posição inicial
   this.draw();
}

/* agora definimos as funções de desenho 
   e movimentação no prototype da nossa particula */

// se voce não entende sobre prototype ainda, não se preocupe
// só tenha em mente que essas funções serão acessiveis pelas nossas particulas
Particle.prototype.draw = function(){
   /* quando vamos fazer qualquer desenho,
      devemos sempre salvar o estado do nosso palco
      e restaurá-lo no final do nosso desenho */
   this.ctx.save();
   
   /* aqui definimos o radius em uma variável para facilitar seu acesso */
   var radius = this.radius;
   
   /* fazemos todos aqueles passos, começar o caminho, desenhar, fechar e completar !*/
   this.ctx.beginPath();
   this.ctx.arc(this.x, this.y, radius, 0, Math.PI*2);
   this.ctx.closePath();
   this.ctx.fillStyle = "#fff"; // a cor da nossa partícula será branca
   this.ctx.fill();
   
   /* restauramos o nosso palco ao estado original, 
      nenhuma alteração de cor, posição da caneta do
      desenho, etc.. será salva */
   this.ctx.restore();
}
Particle.prototype.move = function(){
   /* na nossa função que controla a movimentação 
      nós iremos somar a velocidade da nossa particula
      na posição atual dela, em x e y */
   this.x+=this.vX;
   this.y+=this.vY;
   
   /* após mudar a posição da partícula, "movimenta-la",
      nós podemos*/
   this.draw();
}
</code></pre>
</div>
<p>O código da nossa partícula está pronto !</p>

<h3>O container das partículas</h3>
<p>Essas partículas terão que ser administradas por outro container, e esse será responsável por movimentar todas em conjunto, criá-las, e manter o controle da quantidade máxima que estará vagando pela tela.</p>
<div class="code javascript">
   <span class="file-name">index.js</span>
   <pre><code>
window.onload = draw;
function draw(){
   var canvas = document.getElementById("mycanvas");
   canvas.width = window.innerWidth;
   canvas.height = window.innerHeight;

   /* como iriamos ter que passar o contexto,
      a largura do nosso elemento canvas 
      e a altura do nosso elemento 
      mudamos o código para passar o elemento e 
      dentro da função de inicialização do nosso 
      container, nós obtemos a largura/altura e 
      contexto de desenho */   
   particles.init(canvas);
}

/* criação de um objeto normal */
var particles = {
   /* valores default de algumas variáveis*/
   canvasEl:null,
   width:0,
   height:0,
   /* array que guardará nossas partículas */
   particles:[],
   /* definição da quantidade máxima de particulas */
   maxParticles:150,
   /* nossa função de inicialização responsável por 
      definir os valores das nossas variáveis */
   init:function(canvasEl){
      /* canvasEl é o próprio */
      this.canvasEl = canvasEl;
      /* o width do nosso container é igual ao width do nosso elemento */
      this.width = this.canvasEl.width;
      /* o height do nosso container é igual ao height do nosso elemento */
      this.height = this.canvasEl.height;
      /* obtenção do nosso contexto de renderização */
      this.ctx = this.canvasEl.getContext("2d");
      
      /* depois de setar todas as variáveis, nós mandamos 
         montar o array que guardará as nossas partículas */
      this.mountParticles();
   },
   /* função responsável por montar o array das partículas */
   mountParticles:function(){
      /* na função nós fazemos um loop que criará o número de partículas
         máximo nas posições do nosso array de partículas */
      for(var i = 0; i < this.maxParticles; i++){
         /* para dar um pouco mais de dinamismo às nossas partículas 
            deixaremos todas as variáveis serem definidas aleatóriamente */
         var initialX = Math.random()*this.width; // a posição inicial será algum ponto
         var initialY = Math.random()*this.height;// dentro dos limites do nosso container

         /* as velocidades máximas em x e Y será 2 pixels por frame
            e a direção será definida aleatoriamente por (-1)^numero_aleatorio
            assim a direção poderá ser negativa ou positiva e teremos partículas
            viajando por todos os lados */
         var speedX = 2*Math.random()*Math.pow(-1, Math.floor(Math.random()*10));
         var speedY = 2*Math.random()*Math.pow(-1, Math.floor(Math.random()*3));

         /* "instanciamos" nossa partículas novas passando 
            todos os valores que obtivemos acima como default */
         this.particles[i] = new Particle(this.ctx, initialX, initialY, speedX, speedY);
      }

      /* após criar todas as partículas no nosso array,
         finalmente damos o pontapé inicial no movimento 
         das partículas */
      this.moveParticles();
   },
   moveParticles:function(){
      /* como essa função irá redesenhar todas as partículas
         nós temos que limpar o canvas inteiro, dessa forma não
         irá sobrar nenhum rastro delas pela tela */
      this.ctx.clearRect(0, 0, this.width, this.height);
      // indico tirar uma vez para voce entender o que acontece
      // PS: até que fica legal hahaha

      // interagimos em todas as nossas partículas
      this.particles.forEach(function(p){
         /* e mandamos movimentar uma a uma
            dentro da função move da partícula 
            ela chama a função de desenhar, lembram ? */
         p.move();
      });

      /* usando recursividade nós mandamos mandamos requisitamos
         ao navegador que ele mova todas as partículas novamente
         no próximo frame, se tudo estiver rodando ok e leve
         tende a manter a média de 60 frames por segundo
         mas isso é variável de acordo com a complexidade da
         sua animação */
      this.raf = window.requestAnimationFrame(this.moveParticles.bind(this));
      /* IMPORTANTE passar esse objeto com o bind, se não
         a animação não irá ocorrer pois o valor de this
         dentro da função será diferente */
   }
}

// o resto continua igual 
function Particle(ctx, x, y, vX, vY){ ... }
Particle.prototype.draw = function(){ ... }
Particle.prototype.move = function(){ ... }
</code></pre>
</div>
<p>Se voce seguiu os passos até aqui, voce ja tem uma visualização bem legal na sua tela !</p>
<p data-height="600" data-theme-id="0" data-preview="true" data-slug-hash="apWJqw" data-default-tab="result" data-user="mathmesquita" data-embed-version="2" data-pen-title="Efeito de partículas(incompleto)" class="codepen">See the Pen <a href="http://codepen.io/mathmesquita/pen/apWJqw/">Efeito de partículas(incompleto)</a> by Matheus Mesquita (<a href="http://codepen.io/mathmesquita">@mathmesquita</a>) on <a href="http://codepen.io">CodePen</a>.</p>
<script async src="https://production-assets.codepen.io/assets/embed/ei.js"></script>
<p>Só temos um problema, nossas partículas estão sumindo quando passam da borda da tela. :( </p>

<h3>Mantendo as partículas dentro da tela</h3>
<p>O efeito até ta maneirinho, ta tudo se movendo loucamente pra lados aleatórios, mas depois de alguns segundos, ou minutos pras particulas que deram a sorte de pegarem velocidade quase zero, todas elas somem.</p>
<p>Sendo assim o usuário só conseguiria vê-las no primeiro load da página, não poderia ficar admirando toda a beleza daquele movimento por mais tempo. Tendo esse problema, vamos fazer as modificações no nosso código para mantê-ls na tela.</p>

<div class="code javascript">
   <span class="file-name">index.js</span>
   <pre><code>
window.onload = draw;
function draw(){ ... }

var particles = { 
   ...
   mountParticles:function(){
      for(var i = 0; i < this.maxParticles; i++){
         var initialX = Math.random()*this.width;
         var initialY = Math.random()*this.height;
         var speedX = Math.random()*2*Math.pow(-1, Math.floor(Math.random()*10));
         var speedY = Math.random()*2*Math.pow(-1, Math.floor(Math.random()*10));

         // this.particles[i] = new Particle(this.ctx, initialX, initialY, speedX, speedY);
         /* não precisamos mais passar o context para nossas partículas 
            pois ele será acessível pela cadeia do prototype */
         this.particles[i] = new Particle(initialX, initialY, speedX, speedY);
      }
      this.moveParticles();
   },
   ...
}


function Particle(x, y, vX, vY){ 
   this.radius = 1;
   
   // this.ctx = ctx;
   // não precisamos mais definir o contexto dentro da partícula
   this.x = x;
   this.y = y;
   this.vX = vX;
   this.vY = vY;

   this.draw();
}
/* aqui definimos que o prototype da nossa particula 
   vai ser o nosso container de partículas
   isso nos permitirá ter acesso as propriedades do container
   diretamente pela partícula com o uso do this */
Particle.prototype = particles;
/* essa declaração tem que estar acima das declarações abaixo
   pois será como se estivessemos adicionando mais métodos no 
   nosso prototype, de declararmos abaixo das funções, nós 
   iriamos sobrescreve-las e tudo pararia de funcionar */

Particle.prototype.draw = function(){ ... } // função continua igual

Particle.prototype.move = function(){
   this.x+=this.vX;
   /* precisamos adicionar esses dois ifs
      que irão verificar se a partícula saiu
      da tela e reiniciar a posição para o lado
      oposto, parecendo que ela entrou em um 
      portal em cima e apareceu em baixo
      ou vice-versa */
   if(this.x > this.width) {this.x = 0;}
   if(this.x < 0) {this.x = this.width;}

   this.y+=this.vY;
   /* mesma coisa da esquerda para direita
      ou vice-versa */
   if(this.y > this.height) {this.y = 0;}
   if(this.y < 0) {this.y = this.height;}

   /* NOTEM QUE as propriedades this.width e this.height que 
      estamos consultando, estão sendo herdadas do
      objeto de particles graças ao prototype ! */
   /* lembrando que essas propriedades representam a largura
      e altura do nosso elemento canvas */
   
   this.draw();
}
</code></pre>
</div>
<p>Feitas as alterações acima, temos o nosso efeito de partículas finalizado !!! uhul !!!</p>
<p data-height="600" data-theme-id="0" data-preview="true" data-slug-hash="dNWvQK" data-default-tab="result" data-user="mathmesquita" data-embed-version="2" data-pen-title="Efeito de partículas(incompleto) 2" class="codepen">See the Pen <a href="http://codepen.io/mathmesquita/pen/dNWvQK/">Efeito de partículas(incompleto) 2</a> by Matheus Mesquita (<a href="http://codepen.io/mathmesquita">@mathmesquita</a>) on <a href="http://codepen.io">CodePen</a>.</p>

<h3>Só isso matheus?</h3>
<p>Para aqueles que acharam que ficou legal mas <i>beehhh...</i>, vamos fazer umas modificações e botar uma interação com o mouse na tela ! Assim o usuário irá poder procrastinar no seu site durante horas interagindo com as partículas.</p>
<div class="code javascript">
   <span class="file-name">index.js</span>
   <pre><code>
window.onload = draw;

function draw(){
   var canvas = document.getElementById("mycanvas");
   canvas.width = window.innerWidth;
   canvas.height = window.innerHeight;

   particles.init(canvas);
   
   /* vamos botar um evento de quando o mouse se move */
   canvas.onmousemove = particles.mouseHandler.bind(particles);

   /* e quando ele sair da tela as posições serão "resetadas"
      dessa forma não teremos nenhum bug de linha levando a nada */
   canvas.onmouseleave = particles.mouseHandler.bind(particles, {clientX:-100, clientY:-100});

   /* LEMBRANDO QUE, é importante passar esses métodos 
      com o bind apontando para o objeto particles 
      se não a keyword this não terá a mesma funcionalidade
      dentro da função */
}

var particles = {
   ...
   /* definimos as posições default do mouse em X e Y
      essas posições equivalem a como se ele estivesse
      fora da tela */
   mouseX: -100,
   mouseY: -100,
   ...
   /* todas as outras funções continuam iguais
      só precisaremo adicionar o nosso handler do mouse
      para mudarmos a posição do mouse no nosso objeto */
   mouseHandler:function(e){
      this.mouseX = e.clientX;
      this.mouseY = e.clientY;
   }
   ...
}

function Particle(x, y, vX, vY){ ... }
Particle.prototype = particles;
/* o resto continua igual e precisaremos 
   realizar modificações na função que desenha
   as nossas partículas */
Particle.prototype.draw = function(){
   this.ctx.save();

   var radius = this.radius;
   /* nós adicionamos a variável que mede a distância para o mouse
      dessa forma quando a partícula estiver próxima ao mouse, 
      mais precisamente a menos de 100px, uma linha ligará o mouse
      a partícula, nota-se que estamos acessando as propriedades
      mouseX e mouseY do objeto particles, graças ao prototype chain */
   var distanceToMouse = Math.sqrt(Math.pow(this.mouseX-this.x, 2) + Math.pow(this.mouseY-this.y, 2));
   // condicional para verificar a proximidade
   if(distanceToMouse < 100){
      // se estiver próximo desenha a linha
      this.drawLine(distanceToMouse);
      // e aumenta o tamanho da nossa bolinha
      radius*=3;
   }
   
   // resto continua igual
   this.ctx.beginPath();
   this.ctx.arc(this.x, this.y, radius, 0, Math.PI*2);
   this.ctx.closePath();
   this.ctx.fillStyle = "#fff";
   this.ctx.fill();

   this.ctx.restore();
}
Particle.prototype.drawLine = function(distanceToMouse){
   /* desenho simples de uma linha, seguindo todos
      os passos para se desenhar um caminho */
   this.ctx.beginPath();
   this.ctx.moveTo(this.x, this.y);
   this.ctx.lineTo(this.mouseX, this.mouseY);
   this.ctx.closePath();

   /* aqui definimos a cor da linha para ficar mais 
      transparente quanto mais longe estiver do mouse */
   this.ctx.strokeStyle = "rgba(255, 255, 255, "+(1-(distanceToMouse/100)).toFixed(1)+")";
   /* mandamos desenhar a linha */
   this.ctx.stroke();
}
Particle.prototype.move = function(){ ... } // continua igual
</code></pre>
</div>

<h3>Resultado</h3>
<p data-height="600" data-theme-id="0" data-slug-hash="wgdoyz" data-default-tab="result" data-user="mathmesquita" data-embed-version="2" data-pen-title="Efeito de partículas" class="codepen">See the Pen <a href="http://codepen.io/mathmesquita/pen/wgdoyz/">Efeito de partículas</a> by Matheus Mesquita (<a href="http://codepen.io/mathmesquita">@mathmesquita</a>) on <a href="http://codepen.io">CodePen</a>.</p>

<h3>Conclusão</h3>
<p>É isso aí galera, espero que tenham gostado do efeito, e dessa série, para quem quiser mais posts sobre canvas é só pedir nos comentários, e pra quem se interessou por tudo que fizemos com o javascript é só pedir posts sobre javascript tb nos comentários !</p>
<p>O efeito é simples mas é de coração, existem libraries como o <a href="http://vincentgarreau.com/particles.js/">particles.js</a> que ja tem alguns efeitos bem legais prontos também, lembrando que agora voces ja sabem o caminho das pedras para desenvolverem os próprios efeitos.</p>
<p>Qualquer dúvida, sugestão, critíca, desabafo etc estarei nos comentários ! <b>para quem ficou esperando post meu ontem</b>, peço desculpas pelo meu notebook, meu windows corrompeu e basicamente ficarei 1 mês sem ele :( , sendo assim também não terá post amanhã, mas durante a semana que vem irão rolar alguns posts sim !</p>
<p>Abraço pessoal e até a próxima ! obrigado a quem leu até aqui e nos vemos no próximo post !!</p>
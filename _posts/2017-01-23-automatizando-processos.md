---
layout: post
title: "Automatizando Processos #1"
image: "/assets/automatizando-processos/cover.jpg"
date:   2017-01-23 08:00:00 -0200
color_template: "etc"
tags: etc canvas javascript
resume: "Acredite, esses thumbs não são gerados por nenhuma API legal do facebook, todos são feitos manualmente. Ou pelo menos antigamente eram..."
topic: etc
comments: true
---

<h3>Introdução</h3>
<p>Bom dia, boa tarde e boa noite, vocês talvez estejam se perguntando <i>"o que diabos esse titulo quer dizer?"</i>, se acalmem que irei explicar. No nosso cotidiano, estamos cercados de tarefas chatas que devem ser realizadas sempre do mesmo jeito, nas mesmas métricas, são aquelas tarefas que você não consegue fugir, quando o cliente de liga pedindo pra fazer tal coisa no site pra ele, pois ele contratou o modo manual do sistema, voce ja pensa <i>"la vou eu fazer trabalho de corno de novo..."</i>.</p>
<p>Mas ae eu pensei <i>"e se invés de ficar se lamentando sobre isso não automatizo esse trabalho?"</i>, nessa série <b>que não tem data de término</b> e <b>nem periocidade</b>, eu vou mostrar como resolvi alguns dos meus processos usando tecnologias WEB ! A intenção infelizmente não é fazer tutoriais, mas sim mostrar o meu problema, como eu tinha que fazer, e o que eu fiz para resolver ele. No final os códigos estarão todos no github e voce poderá utilizar até a mesma solução caso tenha um problema semelhante.</p>
<p>Sem mais explicações, vamos ao primeiro problema da série ! <b>Os thumbs do facebook</b></p>

<h3>Problema</h3>
<p>Fazer esses posts são muito legais, <b>de verdade</b>, eu gosto de escrever aqui sobre as coisas que eu faço, ou as idéias que eu tenho, ou até mesmo no caso da <a href="/2016/01/07/bem-vindo-ao-canvas">série do canvas</a>, do <a href="/2017/01/19/menu-toggle-sem-javascript">menu toggle sem JS</a> ou até mesmo dos <a href="/2017/01/17/desenhos-somente-com-css"> desenhos feitos somente com CSS</a>, ensinar algo que eu saiba fazer para vocês.</p>
<p>Mas toda essa parte boa e divertida, também trás algumas coisas que têm que ser feitas, uma delas é o <b>o thumb que irá aparecer na chamada do post no facebook</b>, tipo esse aqui em baixo.</p>
<img src="/assets/automatizando-processos/cover.jpg" alt="">
<p>Quando comecei a fazer o blog, eu tinha imaginado que o <a href="http://ogp.me/">open graph</a> fazia isso para mim, com alguma tag ultra bolada, mas como ja dizia collor <i>é um sonho de uma noite de verão</i>, e eu teria que ter esse <i>corno job</i> de montar a thumb do post sempre que eu terminasse de escrevê-lo.</p>

<h3>Idéias de como solucionar</h3>
<p>A princípio fui na mais básica, montar um templatezinho no inkscape, ou photoshop ou qualquer programa de edição de imagens e toda vez que eu fosse faz um post novo era só entrar lá, mudar o texto, mudar a tag<i>(que eu adicionei recentemente)</i>, a cor de fundo. Nada que uns 3~5 min não resolvam !</p>
<p>Eu até tinha aceitado bem esse método mas lembrei de um pequeno detalhe, <b>eu não sou o às dos editores de imagem</b>, o trabalho que eu iria ter de modificar as coisas toda vez(sim, eu me enrolo para caramba nas ferramentas), iria talvez demorar um pouco mais do que deveria, e pra montar o template ??? nusss... Realmente talvez existisse uma opção melhor.</p>
<p>Eis que então surge minha segunda solução, <i>"bora fazer um plugin pro jekyll que vai gerar isso pra mim e adicionar no template"</i>, essa era uma ótima opção pois teria trabalho zero para gerar esses thumbs, imagina que fácil? só escrever e não ter o trabalho de fazer a thumb depois.</p>
<p>Mas novamente, temos um problema com essa ótima idéia, jekyll foi feito em ruby e não manjo nada de ruby<i>(nunca vi, ja comi eu só ouço falar)</i>, e nem manjo sobre como fazer um plugin para o jekyll. Não que eu não queira aprender, afinal ruby será a próxima linguagem que irei brincar, mas eu precisava disso na hora.</p>

<h3>Solução</h3>
<p>Para firmar conhecimento, eu gosto de utilizar aquilo que aprendi em problemas da vida real, até mesmo porque é bem assim que a vida funciona, voce tem problemas reais e precisa de soluções reais.</p>
<p>No caso do meu problema, eu sabia que com o <b>canvas</b> eu posso gerar imagens, por mais que não tenhamos visto isso na nossa série sobre o canvas, com ele nós podemos utilizar, manipular e criar imagens.</p>
<p>Sendo assim, a única coisa que eu tinha que fazer era utilizar meu conhecimento para fazer um <b>gerador de thumbs de facebook v1.0</b> ! e o resultado disso eu estou compartilhando com vocês para utilizarem caso também precisem de algo semelhante !</p>
<p><a href="https://github.com/MathMesquita/facebookThumbGenerator/blob/master/modelo.html">LINK PARA O GERADOR DE THUMBS. :D</a></p>
<img src="assets/automatizando-processos/uhul.gif" alt="">
<p>PS: preciso dar algumas considerações, eu fiz tudo(css/js) em um único arquivo HTML, dessa forma eu não preciso me preocupar sempre em ter assets comigo, se eu precisar do meu gerador é só dar dois cliques e funciona que nem um programa no browser. Todo o JS está comentado, sendo assim quem tiver interesse é só ler lá, também me mandar uma mensagem caso encontre alguma dúvida !</p>

<h3>Conlusão</h3>
<p>Primeiramente muito obrigado a todos que leram o post até aqui, espero que eu tenha ajudado alguém que estava precisando de algo semelhante e que eu tenha estimulado várias mentes a identificarem um processo e tentarem achar uma solução maneira para automatizá-lo.</p>
<p>Esse post não será o único nesse estilo, pretendo trazer outros posts sobre outras coisas que automatizei, ou boilerplates que criei, <stroke>eu fiz uma parada muito legal para substituir o jquery na minha vida</stroke>, pretendo compartilhar todos com vocês.</p>
<p>Pra quem ficou chateado ao saber que da pra mexer com imagens no canvas eu nem comentei isso na série, se acalmem que está para vir aí um <b>canvas avançado</b>.</p>
<p>Amanhã tem post de novo, e espero todos vocês aqui ! obrigado novamente e estou a espera de duvidas, criticas, sugestões e qualquer outra interação nos comentários ! Passar bem e até a próxima !</p>
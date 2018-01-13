---
layout: post
title: "Por que você ainda usa jquery?"
image: "/assets/porque-jquery/cover.jpg"
date:   2017-01-18 08:00:00 -0200
color_template: "javascript"
tags: javascript reflexao
resume: "Ja parou para refletir se o jquery é realmente necessário no seu site? Um papo sobre como devemos nos preocupar com cada kb."
topic: js
comments: true
---

<h3>Introdução</h3>
<p>Olá pessoal, <b>antes de tudo</b>, gostaria de falar que esse post não tem a intenção de falar que o jquery não é útil, ou que ele não é valioso e nem mereça ser usado.</p>
<p>É apenas um convite a reflexão sobre quais são as razões que nos levam a utilizar o jquery atualmente, e o que isso pode impactar na quantidade de dados que os nossos usuários irão necessitar fazer download.</p>
<p>Afinal, se <b>você</b> é como eu, e possui uma franquia baixa de dados no celular, talvez se preocupe em quantos Kb ou Mb serão consumidos quando você entrar naquele site que tanto gosta, e nós como desenvolvedores devemos nos preocupar em fornecer a melhor experiência pelo menor custo, nesse caso, de dados.</p>

<h3>Por onde começar?</h3>
<p>Essa talvez seja a pergunta mais difícil de se responder, afinal quando estamos acostumados com algo não notamos que isso talvez possa mais nos atrapalhar do que ajudar.</p>
<p>E falo isso por experiência própria, afinal <b>eu</b> assim como vários, quando entraram pro mundo WEB foram jogados direto no colo desse framework, sem nem aprender o javascript de fato. E quando digo aprender JS, to falando aprender mesmo, saber o que são os objetos<strike>(descobrir que no final tudo se resume a objetos)</strike>, um ajax na unha, aprender a fazer inclusive um JQuery próprio... Além de muitas outras coisas que essa linguagem maravilhosa pode nos proporcionar.</p>
<p>Nesse meio todo, acabamos nos acostumando com as facilidades que ele nos proporciona e usamos ele para tudo, literalmente tudo. Nós eramos capazes de fazer os nossos usuários baixarem 32kb de JS por que gostavamos de uma feature que poderia ter sido reescrita em "código nativo" em 1 linha.</p>

<h3>Certo... Mas e aí?</h3>
<p>Realmente, simplesmente jogar as palavras ao vento não adiantam muito, então abaixo vou relatar a <b>minha experiência</b> das coisas que eu fazia com o Jquery porque não sabia como fazer diferente, talvez você também esteja nessa situação e não saiba, mas hoje é o grande dia !</p>

<h4>Obter referência do DOMNode</h4>
<p>Acho que essa era a minha maior desculpa para eu não parar de utilizar o <b>$</b>.</p>
<p><i>$("seletor css")</i> essa linha era tão fácil de ser digitada, que eu aceitava fazer o usuário baixar os 32Kb só pela minha preguiça em fazer da forma correta.</p>
<p>Sim, eu era um péssimo desenvolvedor por pensar dessa forma, mas decidi tomar vergonha na cara e fazer do jeito certo.</p>
<div class="code javascript">
	<span class="file-name">index.js</span>
	<pre><code>
// retorna o elemento que possui o id = ID_DO_ELEMENTO
var meuElemento = document.getElementBy("ID_DO_ELEMENTO");

// retorna os elementos que possuem a class = CLASSES_DOS_ELEMENTOS
var meusElementos = document.getElementsByClassName("CLASSES_DOS_ELEMENTOS");

// retorna os elementos que são tags html iguais a TAG_HTML
var meusElementosPelaTag = document.getElementsByTagName("TAG_HTML");


// Mas eu vou ter que digitar isso tudo pro resto da vida ?
// Bem, tem a opção que ja estavamos acostumados também !!

var elementoAcostumado	 = document.querySelector("seletor css");
// .querySelector() irá selecionar o primeiro elemento
// que aparecer com aquele seletor na sua DOMTree

var elementosAcostumados = document.querySelectorAll("seletor css");
// .querySelectorAll() irá selecionar todos os elementos
// que possuirem aquele seletor
</code></pre>
</div>

<h4>Adicionar eventos nos elementos</h4>
<p>Que nem na hora de selecionar os elementos, para mim era tão fácil usar o <i>.click()</i> que eu aceitava o cara gastar mais grana por mês só pra poder acessar os sites que eu desenvolvia.</p>
<p>Mesmo que eu não usasse todos os outros eventos, como <i>.change()</i>, <i>.resize()</i> etc..., eu conseguia dormir em paz sabendo que meu usuário estava sendo prejudicado por minha preguiça em escrever essa simples linha:</p>
<div class="code javascript">
	<span class="file-name">index.js</span>
	<pre><code>
elemento.addEventListener("click", function(){
	// meu código de desenvolvedor preocupado com o usuário
});
// .addEventListener() é usada para adicionar qualquer evento
// nos seus elementos, vou por o link para a lista completa
// dos eventos nativos e suas explicações
</code></pre>
</div>
<p><a href="https://developer.mozilla.org/en-US/docs/Web/Events">Lista marota dos eventos</a></p>

<h4>Modificar o HTML dentro da tag</h4>
<p>É... quem não gostava, ou gosta, de chamar aquele <i>.html()</i> no Jquery só pra adicionar o teu conteudo ali dentro da tag, ou <i>.text()</i>.</p>
<p>E eu fazia isso tudo, não porque eu era um péssimo desenvolvedor, talvez existissem piores, mas sim porque eu não me preocupava em ser melhor a cada dia como atualmente.</p>
<div class="code javascript">
	<span class="file-name">index.js</span>
	<pre><code>
// E quem diria que para eu ser melhor eu só precisava digitar

element.innerHtml = "&lt;span&gt;meu html&lt;/span&gt;";
	// para modificar HTML

element.innerText = "meu texto";
	// para modificar o texto

// E se você ta preocupado em como vai obter o HTML
// e o texto depois, te dar a outra dica
var meu_html = element.innerHtml;
var meu_texto = element.innerText;
</code></pre>
</div>

<h4>O famoso ajax</h4>
<p>Esse talvez seja o mais comum, porque também o mais chato, ou eu achava ser o mais chato pelo menos.</p>
<p>E por ser chato, eu decidia que meus usuários não teriam acesso nem a uma library um pouco menor, como o <a href="https://github.com/mzabriskie/axios">axios-js</a>.</p>
<p>Eles iriam ter que baixar 32Kb somente para eu usar um POST request em um formulário de contato.</p>
<div class="code javascript">
	<span class="file-name">index.js</span>
	<pre><code>
var requisicao = new XMLHttpRequest(); // inicializo o request

// monitorando o que irá acontecer
requisicao.addEventListener("progress", updateProgressBar, false);
requisicao.addEventListener("load", complete, false);
requisicao.addEventListener("error", failed, false);
requisicao.addEventListener("abort", canceled, false);
// os primeiros argumentos são os eventos
// os segundos são minha função callback para o que irá acontecer
//	  quando o evento for chamado
// os terceiros é para evitar bubbling de eventos na DOMTree

// abre minha conexão magica
requisicao.open("post", "minha_url", true);
// envia a requisição pro servidor
requisicao.send(new FormData(meu_elemento_formulario));
</code></pre>
</div>

<h3>Conclusão</h3>
<p>Acho que nesse post eu destaquei as desculpas mais comuns da galera para utilizarem o jquery, alguma sua estava na lista ? não ? Se não estava, deixe ela nos comentários, talvez eu, ou outra pessoa possa te ajudar a ser um desenvolvedor melhor, o importante é que o primeiro passo você ja deu ao abrir esse blog.</p>
<p>Claro que existem casos, onde você talvez queira compatibilidade com browsers muito antigos<i>(até os que tem menos de 0.1% de uso a nivel mundial)</i>, e o Jquery possa agilizar a entrega do seu projeto, mas aí fica por sua conta refletir se realmente alguem que use um navegador tão atualizado vai ter uma internet tão boa a ponto de você ostentá-la assim.</p>
<p><b>Ninguém é um programador ruim por usar Jquery</b>, eu me considerava ruim pois após aprender sobre JS notei que eu fazia essas atrocidades com meus usuários. </p>
<p>E também nem sempre fazemos isso de propósito, tudo influencia, principalmente o ambiente onde voce começou o seu primeiro estágio/emprego.</p>
<p>O importante é assumir quando erramos, e nos esforçarmos para melhorar, afinal todos aqui querem um emprego bacana, um salário legal e ser reconhecido pelo trabalho impecável que faz.</p>
<p>É isso aí, muito obrigado por lerem até aqui, e quererem ser melhores devs, espero ter ajudado alguém e, como sempre, estou ansioso por comentários, sugestões, duvidas, prometo responder o mais rapido possível !! Obrigado novamente e até o próximo post pessoal !</p>

---
layout: post
title: "Evitando SQL Injection"
image: "/assets/sql-injection/cover.jpg"
date:   2017-01-25 08:00:00 -0200
color_template: "sql"
tags: sql tutorial segurança
resume: "SQL Injection é um dos tipos de ataque mais comuns da WEB, seu sistema está pronto para se proteger? Descubra nesse post."
topic: sql
comments: true
---
<style>
	input{padding: 4px 7px;}
</style>

<h3>Introdução</h3>
<p>Boa noite, boa tarde ou bom dia pessoal ! Hoje como pedido pela leitora Camila, falarei um pouco sobre a técnica de invasão feita por "injeção de SQL", popularmente conhecido como <b>SQL Injection</b>. <i>PS: fica a dica para quem quer pedir tema, eu escuto seus pedidos viu!</i></p>
<p>Começarei pelas tentativas mais comuns, e em quais áreas do site e no final do post eu irei ensinar como evitar que isso aconteça no PHP, que é atualmente a linguagem WEB mais utilizada, e talvez a que mais vacilam com essas brechas de segurança ! Ja mexi em muitos sistemas antigos que, por não serem atualizados, fizeram clientes perderem databases inteiros de dados importantes. =/</p>
<p>No more talk, mãos a obra.</p>

<h3>O que é SQL Injection?</h3>
<p>O SQL Injection, como já diz o nome, vai se aproveitar da maneira como o SQL funciona e alterar a string de busca com algum input do usuário. Literalmente falando, o usuário irá <b>injetar um código sql</b> dentro do seu sql.</p>
<p>Como isso? vamos começar pelo exemplo abaixo de uma "query normal".</p>
<div class="code sql">
	<span class="file-name">query.sql</span>
<pre><code class="sql">
SELECT * FROM `USERS` WHERE `PASS`='$senha' AND `EMAIL`='$email';
-- considere $senha e $email, duas variáveis que vem do PHP
-- e são concatenadas à nossa string de pesquisa
</code></pre>
</div>
<p>Nessa string de pesquisa não existe nenhum erro, quando o usuário entrar com email e senha dele, nós iremos buscar no banco se existe algum usuario e senha com aquele email, caso verdadeiro nossa busca irá retornar aquele usuário e <i>voilà</i>, ele estará conectado.</p>

<h4>Injections 1=1</h4>
<p>Vamos a nossa primeira injeção, <b>1 = 1</b>, todos que estão lendo concordam que um é igual a um correto? Baseado nessa condição, <b>que retornará sempre verdadeiro</b>, usuários mal intencionados conseguem acessar áreas que existam login simplesmente injetando um pedaço de código SQL no input de email, <strike>considerando que o campo de senha passará por uma função hash</strike>.</p>
<p>Ae voce rebate, <i>"grandes coisa, ele vai por 1=1 caindo dentro das aspas e não vai acontecer nada"</i>, se voce pensou isso sinto lhe informar que é um terrível engano, sabendo como funciona a engine do sql o invasor ja sabe(ou pelo menos imagina) que o campo email é um campo varchar e possui aspas na comparação, pois voce passará uma string.</p>
<p>Dessa forma o pedaço de código que ele põe no campo de email será algo parecido como.</p>
<input type="text" value="' or ''='">
<p>Dessa forma quando essa string do email for concatenada com a string da nossa query, resultará em:</p>
<div class="code sql">
	<span class="file-name">query.sql</span>
	<pre><code>
SELECT * FROM `USERS` WHERE `PASS`='um_hash_qualquer' AND `EMAIL`='' or ''='';
-- ''='' retornará sempre true, então todos os usuários serão resgatados 
--		 nessa query, simulando um login bem-sucedido !
</code></pre>
</div>
<p>Se fosse algum campo numérico, como a busca de algum código por exemplo, o usuário poderia fazer a mesma coisa usando exatamente o 1=1.</p>
<input type="text" value="0 or 1=1">
<div class="code sql">
	<span class="file-name">query.sql</span>
	<pre><code>
SELECT * FROM `PRODUCTS` WHERE `ID`=0 or 1=1;
-- 1 = 1 retornará true, então o usuário terá acesso a todos os 
--		 produtos do seu banco de dados.
</code></pre>
</div>
<p>Realmente, talvez obter a listagem de produtos não irá causar tanto estrago no seu sistema mas isso abre brecha para o próximo tipo de injeção.</p>

<h4>Injections de SQL statements</h4>
<p>Essa injeção irá adicionar um código SQL no meio da nossa query, acabando com o comando anterior usando ponto e vírgula, o invasor bota um comando para ser executado logo depois pelo sql.</p>
<p>Vamos continuar na última query na tabela de produtos. O que aconteceria se o invasor botasse essa entrada no campo numérico?</p>
<input type="text" value="0; SELECT * FROM USERS">
<p>Se voce respondeu que ele irá obter toda a listagem de usuários, parabéns, o invasor acabou de conseguir a lista de todos os seus clientes, e também, caso você não salvasse as senhas usando uma <a target="_blank" href="https://pt.wikipedia.org/wiki/Fun%C3%A7%C3%A3o_hash">função hash</a> como md5, ou mesmo utilizando o <a target="_blank" href="http://php.net/manual/pt_BR/function.md5.php">md5</a> mas sem um prefixo SALT, ele conseguiu as senhas dos seus usuários também.</p>
<div class="code sql">
	<span class="file-name">query.sql</span>
	<pre><code>
SELECT * FROM `PRODUCTS` WHERE `ID`=0; SELECT * FROM USERS;
-- fechando o comando anterior com ';' o usuário malicioso
--	  consegue fazer uma busca por todos os campos da tabela USERS
</code></pre>
</div>
<p>E isso estamos pensando em um invasor bonzinho que só quer roubar as informações dos seus usuários. Porque ele poderia muito bem fazer isso:</p>
<input type="text" value="0; DROP TABLE USERS">
<div class="code sql">
	<span class="file-name">query.sql</span>
	<pre><code>
SELECT * FROM `PRODUCTS` WHERE `ID`=0; DROP TABLE USERS;
-- O invasor acabou de derrubar a tabela de todos os usuários do seu sistema
</code></pre>
</div>
<p>Se voce não tiver um backup, realmente estará em maus lençois, afinal toda a tabela de usuários do seu site foi derrubada, perdeu todos os clientes em uma "simples tentativa de login".</p>

<h3>Maneiras de se proteger</h3>
<p>Existem coisas básicas que voce pode fazer para se proteger de tais tentativas, realizar a validação dos campos sempre através de <a target="_blank" href="https://en.wikipedia.org/wiki/Regular_expression">Regexp</a>(expressões regulares), assim o usuário malicioso não poderá botar um ponto e virgula ou uma aspas no campo de e-mail por exemplo, ou um comando inteiro em um campo que só espera números.</p>
<p>Lembrando que essas verificações devem acontecer também no lado do servidor, afinal o javascript qualquer um consegue editar ou injetar um novo no seu site.</p>

<h4>mysqli no PHP</h4>
<p>Se voce ainda utiliza as funções da lib <a target="_blank" href="http://php.net/manual/pt_BR/book.mysql.php">mysql()</a>, <b>CORRA</b>, porque além de ela estar <i>deprecated</i>, ela é totalmente aberta a invasões desse tipo. Nela, todas as queries são passadas manualmente, como se ocorresse nos exemplos acima de concatenação de strings, logo os ataques ocorrem <b>exatamente igual</b> o que vimos acima.</p>
<p>Mas claro que eu não falaria para voce correr dessa lib sem te dar uma alternativa, a classe <a target="_blank" href="https://secure.php.net/manual/pt_BR/book.mysqli.php">mysqli</a>, ela usa parametros em "bind" para não haver nenhuma concatenação direta, e nesse meio do <i>bind</i> a classe trata o input para nada funcionar como uma injeção.</p>
<p>Por exemplo, caso o usuário entrasse com o código acima mostrado no email, a query resultado do <a target="_blank" href="http://php.net/manual/pt_BR/mysqli-stmt.bind-param.php">bind_param()</a>no nosso <i>statement</i>, será:</p>
<input type="text" value="' or ''='">
<div class="code sql">
	<span class="file-name">query.sql</span>
	<pre><code>
SELECT * FROM `USERS` WHERE `PASS`='um_hash_qualquer' AND `EMAIL`=''' or ''=''';
-- o resultado da string de query irá por todo o conteudo dentro das aspas
--	  independente de ele ja ter aspas ou não
</code></pre>
</div>
<p>Botando todo o conteúdo dentro das aspas, a pesquisa do usuário irá procurar literalmente pelo email que corresponda à <i>'' or ''=''</i>, como sabemos que não existe nenhum email assim, nada será encontrado no banco de dados e a invasão terá sido mal sucedida.</p>

<h3>Código php</h3>
<p><i>"Muito show Matheus, mas e o código dessa bagaça ae?"</i>, para quem se perguntou isso, irei mostrar brevemente como funciona o código e deixar a documentação da classe Mysqli em baixo.</p>
<div class="code php">
	<span class="file-name">index.php</span>
	<pre><code>
&lt;?php 

// aqui instanciamos a classe mysqli, passando como parametro
// o servidor do db, user do db, senha do db, e o nome do db
$mysqli = new mysqli('servidor', 'usuario', 'senha', 'database' );

// com a classe instanciada, nós chamamos o método prepare
// para "preparar" a nossa query que receberá os valores
// LEMBRANDO QUE onde nós botarmos "?"(interrogação) será os locais 
// que entrarão os nossos valores
// ps: não precisa se preocupar com aspas no caso de strings
$stmt = $mysqli->prepare("SELECT name, tel FROM USERS WHERE email=? AND senha=?");

// depois de nosso statement estar preparado com a string da query e as interrogações
// nós iremos definir quais variaveis entram em quais lugares, e seus tipos
// através da função ->bind_param(string de tipos, ...variaveis em ordem da interrogação )
$stmt->bind_param('ss', $email, $senha);
// ps: a "string de tipos" que é o primeiro parametro é composta pela inicial dos tipos
//     das variáveis que estão sendo adicionadas. 

// Os tipos permitidos são: 
//    i  -  variaveis inteiras
//    d  -  variaveis double
//    s  -  variaveis string
//    b  -  variaveis que fornecem dados para um blob

// após definirmos quais variáveis entraram em quais lugares, 
// nós executamos nosso statement
if($stmt->execute()){

   // outra função legal do statement é que podemos fazer
   // algo parecido com o processo inverso do bind_params
   $stmt->bind_result($name, $tel);
   // a função bind results, irá atribuir os valores obtidos 
   // nas queries, no nosso caso name e tel, a variáveis que nós 
   // escolhemos os nomes, eu preferi manter o nome, mas poderia
   // ter posto $var1 e $var2, $var1 receberia o nome e 
   // $var2 receberia o telefone

   // E aqui pegamos todos os resultados imprimindo eles na tela
   // com os nomes das variáveis que definimos no bind_result()
   while($stmt->fetch()){
      echo "nome do usuarrio: $name \n tel do usuario: $tel\n";
   }
}

?>
</code></pre>
</div>
<p>Esse é o código mais básico, mostrei ele somente para vocês terem uma idéia de como funciona o processo para manter as strings de queries limpas. Falta tratamento de erro e outras coisas, sendo assim o que mais indico é dar uma estudada na classe <a href="https://secure.php.net/manual/pt_BR/book.mysqli.php">mysqli</a> e fazer os seus testes !</p>

<h3>Conclusão</h3>
<p>Espero que tenham gostado do post, ele acabou sendo mais explicativo do que técnico porque muita gente ainda não sabe como funciona as injeções de sql e por isso deixam brechas abertas para pessoas mal intencionadas.</p>
<p>Existem implementações de coisas parecidas em várias linguagens, e praticamente todos os frameworks famosos de PHP fazem isso sob os panos. Inclusive se voce ainda desenvolve sem um framework PHP(claro que nem sempre é necessário), indico dar uma estudada neles, agilizam muito o trabalho e em proejtos grandes te trazem uma segurança bem maior do que voce ter que se preocupar com tudo na unha. Eu uso o <a target="_blank" href="https://laravel.com/">Laravel</a>, mas existem outros como <a target="_blank" href="https://framework.zend.com/">ZendPHP</a>, <a target="_blank" href="https://cakephp.org/">cakePHP</a>, <a target="_blank" href="http://www.yiiframework.com/">Yii</a>, <a target="_blank" href="https://codeigniter.com/">CodeIgniter</a> e outros vários até perder de vista.</p>
<p>Obrigado a todos que gastaram o seu precioso tempo lendo até aqui e lembrem que qualquer dúvida, sugestão<i>(como esse post)</i>, crítica, pode deixar nos comentários ! O post fica por aqui e até a próxima ! :D</p>
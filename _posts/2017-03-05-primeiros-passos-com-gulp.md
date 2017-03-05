---
layout: post
title: "Primeiros Passos com Gulp"
image: "/assets/gulp/cover.jpg"
date:   2017-03-05 08:00:00 -0200
color_template: "javascript"
tags: gulp javascript básico
resume: "Gulp explicado de forma simples e com um exemplo que ja pode ser aplicado no seu cotidiano utilizando sass."
topic: javascript
comments: true
---

### Introdução

Bom dia, boa tarde e boa noite ! Como falado no [post anterior](https://mathmesquita.me/2017/03/01/higher-order-components-no-react.html), agora vou focar nas ferramentas que utilizamos no cotidiano de um desenvolvedor front-end. A primeira delas será o [GULP](http://gulpjs.com/), uma *ferramenta de automatização de processos* que irá facilitar **MUITO** sua vida, mesmo no inicio da aprendizagem.

Sei que aprender algo pode ser chato as vezes, principalmente nesses casos que talvez estejamos *"cômodos"* com a maneira que fazemos as coisas, mas garanto que irá valer a pena, ao terminar esse post você ja conseguirá montar o setup básico pra não ter que se preocupar em ligar os pre-processadores, minificar os arquivos etc.

Vamos por as mãos na obra começando pelo que é o **gulp**.

### O que é o gulp ?

Como foi falado no primeiro parágrafo do post, o gulp é uma **ferramenta de automatização de processos**, só isso ja diz muito sobre ele, pra quem não viu meu post sobre [automatização de processos](https://mathmesquita.me/2017/01/23/automatizando-processos.html), lá expliquei que os *processos* são essas tarefas repetitivas chatas mas que temos que fazer durante nosso dia, no post usei o exemplo do meu [gerador de thumbs do facebook](https://github.com/MathMesquita/facebookThumbGenerator/blob/master/modelo.html), mas esse processo era algo muito específico e que não é comum a todos.

Existem outros processos na vida de um desenvolvedor front-end que são comuns à muitas pessoas, tipo quando você tem que passar seu arquivo sass em um pre-processador ou até mesmo minificar seu javascript. 

Pensando nesses casos criaram o **gulp**, utilizando ele, você não precisará mais se preocupar em ligar diversos watchers, um pro sass, outro pra alguma *template engine* etc. Você terá um unico arquivo que com um simples comando irá realizar todo esse trabalho para você, e pela facilidade que é adicionar outros procedimentos em um arquivo, garanto que depois de um tempo suas entregas serão bem melhores e otimizadas.

### Instalando o gulp

Antes de tudo precisaremos do [node](https://nodejs.org/en/) instalado na sua máquina, se você não tem, é só baixar ele [nesse link](https://nodejs.org/en/download/) que ta tudo certo. O node irá instalar o **npm** para nós, que é o **node package manager**, ele é responsável por instalar os nossos pacotes tal como o **gulp** ou o *pre-processador de sass* que utilizaremos.

No mesmo caso do **gulp**, existem concorrentes do *npm* e é um deles que utilizaremos, não se preocupe que para instalar o [yarn](https://yarnpkg.com/pt-BR/) não precisaremos ir em nenhum outro link, só será necessário baixar ele pelo próprio **npm**, pois o yarn além de ser considerado um *gerenciador de pacotes* ele também é um pacote, só rodar **npm i -g yarn** e deixar que o npm faça isso pra você, *claro que nesse momento seu node ja deve estar instalado*.

Crie uma pasta no seu pc, navegue até ela pelo seu terminal e rode os comandos abaixo, lembrando que após rodar **yarn init**, é só sair apertando enter pois esssas informações não irão impactar nosso exemplo.

<div class="code">
<pre><code>
> yarn global add gulp-cli
> yarn init
> yarn add gulp --dev

</code></pre>
</div>

Nossos principais pacotes estão instalados e nosso projeto iniciado, agora só é necessário criar o arquivo *gulpfile.js* na raiz desse diretório que estamos utilizando pro projeto.

### Começando do basicão

Em nosso arquivo **gulpfile.js** que está na raiz do projeto, vamos começar a montar as configurações do nosso automatizador, começando pelo mais bobo que é copiar os arquivos de um **diretório com os fontes** para um **diretório de desenvolvimento**, sem passar por nenhum pré-processador nem nada, simplesmente copiando os arquivos de uma pasta, e jogando eles em outra.

<div class="code javascript">
    <span class="file-name">gulpfile.js</span>
<pre><code>
// primeiro passo é obtermos a referencia do nosso pacote gulp
var gulp = require('gulp');

// criamos a task 'default', que é a task chamada ao rodarmos o 
//    comando gulp no terminal sem nenhum argumento
gulp.task('default', function(){
    // essa task deverá retornar o resultado das operações que 
    //    realizarmos em nossos arquivos que são definidos na 
    //    função .src()
    return gulp.src("src/**/*")
        .pipe(gulp.dest("dev/"));
    // cada .pipe() que aplicarmos na fila de processamento
    //    dos arquivos, é uma operação que vamos realizar neles
    //    por exemplo minificar, autoprefixar etc.
    
    // o último pipe deve ser o glob de destino dos arquivos
    //    no nosso caso a pasta dev/ na raiz do projeto
})

</code></pre>
</div>

Nesse momento, se você criar algum arquivo na pasta **src** e rodar **gulp** no terminal, você verá que esse arquivo será copiado para a pasta dev.

### Separando os arquivos e processando

Acho que copiar os arquivos de uma pasta para botá-los em outra tem pouca utilidade, sendo assim vamos começar a fazer algo com nossos arquivos.

<div class="code javascript">
    <span class="file-name">gulpfile.js</span>
<pre><code>
var gulp = require('gulp');

// rode o comando 'yarn add gulp-sass --dev' no terminal antes
var sass = require('gulp-sass');

// primeiro vamos separar a task do html da task do sass

// a nossa task html não fará nada demais, só irá copiar
//    os arquivos html da pasta src para a pasta dev
gulp.task('html', function(){
    // essas duas estrelas no diretório indicam que é
    //   para vasculhar todos os diretórios a procura
    //   de arquivos com a extensão .html
    return gulp.src('src/**/*.html')
        .pipe(gulp.dest('dev/'));
});

// a task sass irá procurar por todos os arquivos sass
//    dentro de todos os diretórios, e passar nosso
//    pré-processador neles para então salvar o resultado
//    no diretorio dev
gulp.task('sass', function(){
    // seleciona todos os arquivos sass em todos os diretorios
    return gulp.src('src/**/*.sass')
        // passa o pré-processador nos arquivos selecionados
        .pipe(sass())
        // salva eles na pasta dev
        .pipe(gulp.dest('dev/'));
        // extensão .css será adicionada automaticamente
});

// nossa task default irá chamar nossas outras duas tasks a partir de agora
gulp.task('default', [ 'html', 'sass' ]);

</code></pre>
</div>

Feito essas alterações, caso você rode *gulp* no terminal e tenha arquivos html/sass no seu diretório **src** você verá que os htmls serão copiados para pasta **dev** e os arquivos sass serão pré-processados e o resultado estará em css na pasta **dev**. **Lembrando que o pacote do gulp-sass deve ser instalado** através do comando **yarn add gulp-sass --dev**.

### Assistindo mudanças nos arquivos

Infelizmente com as configurações que fizemos até agora, ainda somos dependentes de em toda alteração existir a necessidade de rodar *gulp* no terminal para aplicar essas mudanças e podermos atualizar o resultado na tela do navegador.

O gulp nos possibilita configurarmos um *'watcher'* para assim que acontecer alguma mudança em algum arquivo, ele automaticamente realize o procedimento que aquele tipo de arquivo precisa, *no caso de um arquivo sass*, precisará passar pelo pré-processador novamente.

<div class="code javascript">
    <span class="file-name">gulpfile.js</span>
<pre><code>
var gulp = require('gulp');
var sass = require('gulp-sass');

gulp.task('html', function(){
    return gulp.src('src/**/*.html')
        .pipe(gulp.dest('dev/'));
});

gulp.task('sass', function(){
    return gulp.src('src/**/*.sass')
        // precisamos adicionar um tratamento de erro
        //   no nosso pre-processador, caso contrario
        //   sempre que ocorrer algum erro, nosso watcher
        //   será desligado, e precisaremos religa-lo manualmente
        .pipe(sass().on('error', sass.logError))
        .pipe(gulp.dest('dev/'));
});

// vamos adicionar a task watch, que irá ver se ocorreu
//    alguma mudança nos arquivos html e sass, e irá 
//    chamar as respectivas tasks de cada tipo de arquivo
gulp.task('watch', ['html', 'sass'], function(){
    // essa task não precisará retornar nada pois não estamos
    //    realizando alterações em nenhum arquivo
    
    // todo novo "watcher" que é o responsável por
    //    assistir os arquivos e ver se ocorreram mudanças
    //    deverá ser adicionado dentro dessa task
    gulp.watch('src/**/*.html', ['html']);
    gulp.watch('src/**/*.sass', ['sass']);
    // lembrando que o primeiro argumento é o glob responsavel
    //    por selecionar os arquivos, e o segundo argumento são  
    //    as tasks que devemos chamar caso alguma modificação seja 
    //    detectada 
})

gulp.task('default', [ 'html', 'sass' ]);

</code></pre>
</div>

Se você seguiu os passos corretamente e agora rodar **gulp watch** no seu terminal, você verá que toda modificação que realize em algum arquivo será automaticamente replicada no arquivo destino na pasta **dev**.

### Adicionando outros procedimentos

Para adicionar procedimentos a serem feitos nos arquivos é muito fácil, primeiro você [procura um plugin gulp](http://gulpjs.com/plugins/) que realize o que você deseja fazer, e depois é só botá-lo na fila da task. 

Vamos por exemplo adicionar um **sourcemap** para facilitar nosso debug no sass, e um **autoprefixer** de css responsável por adicionar as tags -webkit-, -moz-, -o- etc, para nós.

<div class="code javascript">
    <span class="file-name">gulpfile.js</span>
<pre><code>
var gulp = require('gulp');
var sass = require('gulp-sass');

// Lembrando que é necessário instalar os dois plugins abaixo

// yarn add gulp-sourcemaps --dev
var soucemap = require('gulp-sourcemaps');
// yarn add gulp-autoprefixer --dev
var autoprefixer = require('gulp-autoprefixer');


gulp.task('html', function(){
    return gulp.src('src/**/*.html')
        .pipe(gulp.dest('dev/'));
});

gulp.task('sass', function(){
    return gulp.src('src/**/*.sass')
        // aqui iniciamos nosso sourcemap, para gravar a posição
        //   inicial das classes e etc em nossos arquivos sass
        .pipe(sourcemap.init())
        // realizamos as alterações no arquivo sass, transformando em css
        .pipe(sass().on('error', sass.logError))
        // adicionamos os prefixos no css gerado
        .pipe(autoprefixer())
        // finalizamos nosso sourcemap, falando que ele deverá 
        //   escrever o arquivo no mesmo diretório que o css gerado
        .pipe(sourcemaps.write('.'))
        // por fim mandamos o resultado ser posto no diretório dev
        .pipe(gulp.dest('dev/'));
});

gulp.task('watch', ['html', 'sass'], function(){
    gulp.watch('src/**/*.html', ['html']);
    gulp.watch('src/**/*.sass', ['sass']);
})

gulp.task('default', [ 'html', 'sass' ]);

</code></pre>
</div>

Alterações feitas, ao rodar o comando **gulp watch** a task *watch* será chamada e a brincadeira irá começar, todo sass que escrevermos será pré-processado e auto-prefixado para a pasta **dev**.

### Conclusão

Espero que tenham gostado do post, e **fica ai a lição de casa**: procure plugins para adicionar na sua fila de procedimentos, procure um [plugin de minificar css para gulp](https://www.npmjs.com/package/gulp-cssnano), ou algum [plugin de alguma template engine do momento](https://www.npmjs.com/package/gulp-pug), para você "templatezar" seu html, *go ahead* e brinque com o gulp, a melhor maneira de aprender algo é praticando.

Pretendo fazer um post **Segundos passos com o gulp** e ensinar como montar um serverzinho com hot-reload para agilizar nosso desenvolvimento, além de falar um pouco mais sobre o tanto de coisa que podemos fazer, como comprimir imagens e avisar que determinados diretórios não precisam ser processados, como um diretório de *includes* por exemplo.

Muito obrigado para quem leu até aqui, é sempre uma honra escrever e transferir conhecimento para vocês, qualquer dúvida, problema, sugestão ou critica, eu estarei nos comentários ! Muito obrigado galera e até o próximo post !! Abraços ! :D
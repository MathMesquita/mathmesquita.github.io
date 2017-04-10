---
layout: post
title: "Code Splitting & Lazy Loading no React"
image: "/assets/splitting/cover.jpg"
date:   2017-04-09 08:00:00 -0200
color_template: "react"
tags: React javascript avançado
resume: "Um manual para você começar a aplicar code splitting e lazy loading no seu app React imediatamente!"
topic: react
comments: true
---

### Introdução

Bom dia, boa tarde ou boa noite, após duas semanas ausente estou de volta ! Novamente tive problemas com meu queridinho notebook. <3

No post de hoje vamos falar novamente sobre *performance*, você conhece a técnica de **code splitting**? Ja ouviu falar mas achava algo muito difícil então nunca tentou implementar? Acreditaria em mim se eu falasse que é algo extremamente simples e que você irá se sentir culpado de não ter feito isso por esse tempo todo?

Nós vamos cobrir o básico da técnica, explicando o problema que ela resolve e um exemplo para você ja aplicar agora em poucas linhas de código. Mais pra frente virá outro post mais avançado sobre como *fazer o split de componentes inteiros* acompanhado de uma reflexão no melhor estilo [estado global & estado local](https://mathmesquita.me/2017/03/19/estado-global-estado-local.html) sobre até que nível é indicado utilizarmos a ténica, pois como sempre falam tudo em excesso faz mal né.

Dedos a obra ! 

### O Problema

Se você utiliza o [create-react-app](https://github.com/facebookincubator/create-react-app), já notou que ao executar o comando *yarn build* o webpack ele pega **TODO** o seu javascript e joga ele em um arquivo *bundle.js*? Fazendo uma analogia com carros, ao gerar o *bundle*, você talvez possa até pensar que ele esteja algo parecido com isso:

![Imagem de um carro potente com uma pessoa dentro](/assets/splitting/carro.jpg)

Algo elegante com todos seus componentes, libraries etc arrumadinhos ali dentro, esse grupo todo muito veloz e sem problemas, afinal está funcionando e talvez na sua internet ele esteja carregando até bem rápido.

Infelizmente como ja dizia Joseph Climber, *"a vida é uma caixinha de surpresas"*, e esse *bundle.js* gerado na verdade está algo semelhante à:

![Imagem de um carro antigo lotado de pessoas, até o teto](/assets/splitting/superlotado.jpg)

Pois é... por mais que o webpack faça automaticamente várias otimizações, infelizmente ele não faz milagre e você acaba mandando para o seu usuário toda a sua aplicação de uma vez só, sendo que todas as vezes que ocorrerem alguma modificação em algum componente não será possível nem aproveitar o *cache via hash* das suas libraries mais comuns.

### Code Splitting

Eis que chega o salvador da nosssa pátria, a técnica de **code splitting** irá separar esse seu arquivo gigante em vários arquivos menores, possibilitando inclusive de só baixarmos eles quando for necessário, que é o que chamamos de **Lazy Loading**.

Antes de pularmos para o **Lazy Loading** vamos primeiro ver como realizar o *split* mais básico dos nossos arquivos.

### Vendor e App

O arquivo **vendor** geralmente é onde botamos todas as nossas dependencias de terceiros, esse é o primeiro nível de *split* que podemos aplicar no nosso app, ao separarmos o *vendor* nós podemos melhorar o cache do mesmo com uma **hash** no nome do arquivo indicando se ele foi alterado ou não comparado ao último request.

Como não é algo comum dar update em libraries de terceiros, esse arquivo de vendor irá ficar durante bastante tempo sem ser alterado e o browser irá servir um cacheado para nosso usuário, reduzindo assim o tempo de loading da página.

#### Separando os arquivos

Considerando que você utilizou *create-react-app*, você precisará primeiro executar **yarn run eject** e após isso abrir o arquivo *webpack.config.dev.js*, esse arquivo é o responsável por inicializar nosso *hot-reload-server* e gerar as builds que vemos em desenvolvimento.

Tudo o que vamos fazer de alterações no arquivo de desenvolvimento devem ser replicadas no arquivo *webpack.config.prod.js*, apesar de não ser obrigatório essas mudanças no arquivo de desenvolvimento, iremos realizá-las para ficar mais fácil a visualização do que ocorre em nosso app.

Primeiro vou copiar aqui trechos do código atual que vocês encontrarão, e depois mostrarei as alterações nesses trechos.

<div class="code javascript">
  <span class="file-name">webpack.config.dev.js</span>
<pre><code>
...
module.exports = {
  ...
  // todos os comentários foram removidos
  entry: [
    require.resolve('react-dev-utils/webpackHotDevClient'),
    require.resolve('./polyfills'),
    paths.appIndexJs
  ],
  ...
  output: {
    path: paths.appBuild,
    pathinfo: true,
    filename: 'static/js/bundle.js',
    publicPath: publicPath
  },
  ...
  plugins: [
    ...
    new HtmlWebpackPlugin({
      inject: true,
      template: paths.appHtml,
    })
  ]
};

// todos os locais que adicionei "..." significa linhas que foram
// omitidas pois não se relacionarão com nossas alterações

</code></pre>
</div>

Segundo a documentação da configuração [entry](https://webpack.github.io/docs/configuration.html#entry) do **webpack v1**, quando passamos um array como valor todos os arquivos que estão nesse array serão concatenados em um unico arquivo de saída, dessa forma o primeiro passo que iremos realizar será o de separar os arquivos do **servidor de desenvolvimento**, do **polyfill** e do nosso **app**.

Em **output**, como agora teremos vários arquivos diferentes na nossa entrada, precisaremos deixar dinâmico a criação desses arquivos pois da maneira que está todos ele irão se chamar **bundle.js** e irão ser alocados em **static/js**.

Por último em **plugins** iremos adicionar um plugin do próprio webpack chamado [Commons Chunk Plugin](https://webpack.github.io/docs/list-of-plugins.html#commonschunkplugin), nele nós realizaremos as configurações para separar todos os modulos que forem de terceiros, ou seja, instalados em nosso projeto.

<div class="code javascript">
  <span class="file-name">webpack.config.dev.js</span>
<pre><code>
...
module.exports = {
  ...
  // Removemos o array e agora passamos um objeto
  // que o nome de cada `key` será a propriedade
  // `name` em output.
  entry: {
    devServer:[
      require.resolve('webpack-dev-server/client') + '?/',
      require.resolve('webpack/hot/dev-server'),
    ],
    polyfills:[
      // vamos deixar esse arquivo de polyfills separado
      // pois também não será alterado com frequência
      // e é extremamente simples
      require.resolve('./polyfills')
    ],
    app:path.join(paths.appSrc, 'index'),
  },
  ...
  output: {
    path: paths.appBuild,
    pathinfo: true,
    // `[name]` irá concatenar o nome do arquivo que passamos
    // em entry, como devServer || polyfills || app
    filename: 'static/js/[name]-[chunkhash:8].js',
    // `[chunkhash:8]` irá concatenar um código hash de 8 caracteres
    // que representa o conteúdo do arquivo, assim o browser
    // conserguirá saber se o arquivo foi alterado ou não
    chunkFilename: 'static/js/[name]-[chunkhash:8].js',
    // a propriedade chunkFilename é o nome que será dado
    // aos chunks que serão gerados toda vez que fizermos algum
    // split dentro da nossa aplicação, com intenção de realizar
    // lazy loading
    publicPath: publicPath
  },
  ...
  plugins: [
    ...
    // aqui não alteraremos nada anterior, só adicionaremos
    // esse plugin no array
    new webpack.optimize.CommonsChunkPlugin({
      // nome do bundle que iremos gerar
      name: 'vendor',
      // função que verifica cada pacote que importarmos
      // no nosso app, caso ele pertença a pasta `node_modules`
      // significa que ele é um módulo de terceiros e irá ser
      // adicionado ao arquivo de vendor
      minChunks: ({ resource }) => /node_modules/.test(resource),
      // nome do arquivo final gerado, `nome_do_bundle.hash.js`
      filename: 'static/js/[name].[chunkhash:8].js'
    }),
    // muda nada
    new HtmlWebpackPlugin({
      inject: true,
      template: paths.appHtml,
    })
  ]
};

// todos os locais que adicionei "..." significa linhas que foram
// omitidas pois não se relacionarão com nossas alterações

</code></pre>
</div>

Após essas modificações, se você reinicializar seu app e ir na aba **network** do seu debugger, verá que seus arquivos já estão sendo carregados separadamente.

O único problema que temos nas configurações iniciais ainda do **create-react-app** é que os *bundles* gerados são adicionado ao nosso html sem a tag [async ou defer](http://www.growingwiththeweb.com/2014/02/async-vs-defer-attributes.html), precisamos instalar um plugin para adicionar a tag *defer*, pois nossos arquivos precisam ser baixados sem interromper o *html parser* e devem ser executados na ordem em que aparecem em nosso DOM, primeiro o *vendor* e depois os demais.

Para isso nós utilizaremos o plugin [script-ext-html-webpack-plugin](https://github.com/numical/script-ext-html-webpack-plugin), instale esse plugin como uma dependência de desenvolvedor e aplique as seguintes alterações no seu arquivo webpack.

<div class="code javascript">
  <span class="file-name">webpack.config.dev.js</span>
<pre><code>
// temos que importar nosso plugin
var ScriptExtHtmlWebpackPlugin = require('script-ext-html-webpack-plugin');

...
module.exports = {
  ...
  plugins: [
    ...
    new webpack.optimize.CommonsChunkPlugin({
      name: 'vendor',
      minChunks: ({ resource }) => /node_modules/.test(resource),
      filename: 'static/js/[name].[chunkhash:8].js'
    }),
    new HtmlWebpackPlugin({
      inject: true,
      template: paths.appHtml,
    }),
    // e adicioná-lo na nossa lista de plugins
    new ScriptExtHtmlWebpackPlugin({
      defaultAttribute: "defer"
    })
    ...
  ]
};

// todos os locais que adicionei "..." significa linhas que foram
// omitidas pois não se relacionarão com nossas alterações

</code></pre>
</div>

Se você deu uma breve lida na documentação do plugin, provavelmente viu que ele **funciona em conjunto** com o *HtmlWebpackPlugin* e que simplesmente serve para adicionarmos essas tags nos scripts injetados em nosso html.

### Lazy Loading

A técnica **Lazy loading** consiste em entregarmos pro usuário somente o que ele irá usar, assim quando ele estiver na *rota index* por exemplo, ele baixará somente os arquivos que são utilizados na rota index. 

Caso o usuário mova de uma rota para outra, ele já terá os arquivos da rota anterior e precisará baixar somente os arquivos da nova rota, e como já separamos o *vendor* da *aplicação*, os arquivos baixados irão ser bem pequenos, pois terão somente o código do componente que eles pertencem.

#### Lazy Loaded Routes

Estamos utilizando **React Router v3** e **Webpack v1**, caso você esteja utilizando **webpack v2** no final eu irei mostrar quais seriam as diferenças.

*"Então Matheus, como fazemos essa bagaça ae???"* No seu arquivo de rotas, você precisará substituir a declaração explícita do componente de determinada rota, por uma função que será responsável por carregar esse componente quando for necessário. Lembrando que o **webpack** cuida de tudo, ele reconhece quando você adicionou um *"split point"* e se ele deve ser carregado de forma *lazy*.

Vamos para hora da verdade, o que deve ser feito de tão complicado no arquivo de rotas !!!

<div class="code javascript">
  <span class="file-name">routes.js</span>
<pre><code>

// Como provavelmente era
import Main from 'Main.js';
...
  &lt;Route path="/" component={Main} /&gt;

// como deve ficar
...
  &lt;Route path="/" getComponent={(_, cb) =&gt; {
    // require.ensure é uma forma do webpack saber
    // que nesse local deve ser criado um split point
    require.ensure([], function(){
      let Main = require('Main.js').default;
      // após carregar os arquivos que serão separados
      // nós passamos o modulo default como segundo parâmetro
      // da função callback do getComponent
      cb(null, Main);
    })
  }} /&gt;
...

// ATENÇÃO, estamos utilizando o
// react-router v3 !!!

</code></pre>
</div>

Viu como é simples? Você deve fazer isso para cada rota sua que deseja ser carregada dessa forma, ou criar uma factory que faça isso para você. O imporante é que agora você não obrigará o usuário a baixar todas as rotas da sua aplicação para poder usá-la.

#### Com Webpack v2

No webpack v2, nós ja temos suporte ao [import()](https://webpack.js.org/guides/code-splitting-import/) **async** que retorna uma promise, lembrando que para utilizar com o **babel** você precisará do seguinte plugin instalado [syntax-dynamic-import](http://babeljs.io/docs/plugins/syntax-dynamic-import/).

Nosso código então ficaria assim:

<div class="code javascript">
  <span class="file-name">routes.js</span>
<pre><code>
...
  &lt;Route path="/" getComponent={(_, cb) =&gt; {
    // com o novo import() async podemos encadear
    // promises para retornar nossa rota
    import('Main.js')
      .then(module =&gt; module.default)
      .then(Component =&gt; cb(null, Component))
      .catch(e =&gt; cb(e, null));
  }} /&gt;
...

// ATENÇÃO, estamos utilizando o
// react-router v3 !!!

</code></pre>
</div>

Bem mais limpo e bonito, caso você queira migrar do *webpack v1* para o *v2*, em breve farei um post sobre como fazer isso! até lá, você pode ir seguindo [esse guia aqui](https://webpack.js.org/guides/migrating/).

### Conclusão

Espero que tenha feito um bom post após quase 3 semanas fora e que vocês tenham gostado, repassar conhecimento é sempre um prazer, principalmente nessas coisas bobas que as pessoas pensam ser tão complicado mas na verdade são bem simples.

Agora que estou com meu notebook voltarei com os posts semanais, talvez um ou outro no meio da semana mas com certeza todo domingo um novo. Semana que vem falarei sobre **Service Workers** !!! outra magia negra nem tão negra assim, e que junto de algumas ferramentas se torna bem fácil de adicioná-lo no seu app.

Caso vocês tenham curtido, podem compartilhar a vontade ! Obrigado pelo seu tempo e qualquer dúvida, critica ou sugestão os comentários estão abertos e eu pronto para respondê-los.

Abraços e até o próximo post !

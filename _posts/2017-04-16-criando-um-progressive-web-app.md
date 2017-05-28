---
layout: post
title: "Criando um Progressive Web App"
image: "/assets/pwa/cover.jpg"
date:   2017-04-16 08:00:00 -0200
color_template: "javascript"
tags: PWA javascript intermediário
resume: Você sabe o que é um PWA? não? Então bora aprender do melhor jeito, fazendo um !
topic: javascript
comments: true
---

### Introdução

Faaaaaaala *garela* ! Bom dia, tarde ou noite, parabéns por ter clicado nesse post e ter se interessado por oferecer uma melhor experiência aos seus usuários, deixando seus web-apps em outro patamar, após você ler o post provavelmente vai utilizar isso em todo app novo que você fizer, e o melhor é que quem irá te agradecer por isso serão seus usuários !

Sabendo sobre <a href="https://developers.google.com/web/fundamentals/performance/optimizing-content-efficiency/http-caching#invalidar_e_atualizar_respostas_armazenadas_em_cache" target="_blank">como funciona o cache</a>, nós ja fazemos alterações no server, adição de hashs para forçar o download de novas versões de nossos arquivos, e separamos nossos files para não precisar ser tudo baixado em conjunto.

Porém, como visto <a href="https://developers.google.com/web/fundamentals/performance/optimizing-content-efficiency/http-caching#definir_a_politica_ideal_de_cache-control" target="_blank">aqui</a>, existem tempos limites para o cache, ou seja se você nao mudar nada na sua página acima desse determinado tempo de cache, o usuário será obrigado a baixar seus arquivos, e dependendo do site isso pode ser algo beeeem demorado.

Tudo isso se soma ao fato de ser uma trabalheira fazer essas modificações, resultando em ninguém usando e quem usa não tendo o resultado tão grandioso quanto era esperado.

Temos então a API <a href="https://www.html5rocks.com/en/tutorials/appcache/beginner/" target="_blank">APP Cache</a>, que diferente do cache em request, era um pouco mais fácil de se implementar porém tinha <a href="https://alistapart.com/article/application-cache-is-a-douchebag" target="_blank">muitos problemas</a>, sério, **MUITOS** só ler a listinha que o rapaz fez no artigo.

### Service Workers

Aí entram os <a href="https://developers.google.com/web/fundamentals/getting-started/primers/service-workers?hl=pt-br" target="_blank">service workers</a>, que te dão um **BOM** controle sobre o que seu usuário irá cachear, e te possibilita outras features muito legais que você com certeza vê vários sites utilizando como <a href="https://developers.google.com/web/updates/2015/12/background-sync?hl=pt-br" target="_blank">background sync</a> e <a href="https://developers.google.com/web/fundamentals/engage-and-retain/push-notifications/?hl=pt-br" target="_blank">notificações de push</a>, que merecem um post só sobre elas.

Tudo isso ocorre por que os service workers funcionam no background da sua aplicação, e logo, conseguem ter acesso a coisas que voce não teria só pelo JS a nível de DOM.

Para seu **service worker** funcionar, além de você precisar estar em <a target="_blank" href="https://jakearchibald.github.io/isserviceworkerready/">um navegador que já tem suporte</a>, você **precisará ter SSL** no seu site **quando em produção**, localhost você conseguirá testar tranquilamente.

Sendo assim, bora codar !

### Criando a mente por trás do cache

Quem tratará de gerenciar todo esse cache para nós será o arquivo *service-workers.js*, e uma coisa **MUITO IMPORTANTE** que devo de falar é que ele deve estar localizado sempre em uma hierarquia maior ou igual ao arquivo que voce deseja cachear por ele.

<div class="code javascript">
  <span class="file-name">service-workers.js</span>
<pre><code>

// esse é o nome que seu cache terá na caches API
var cacheName = 'nome_do_meu_app.v1.0.2';

// aqui você escolhe quais arquivos serão cacheados,
//  basicamente todos os arquivos podem ser cacheados
var filesToCache = [
  '/',
  '/index.html',
  '/scripts/app.js',
  '/styles/meu_css.css',
  '/assets/logo.svg',
];

// evento de INSTALAÇÃO do service worker,
//  nós só conseguimos salvar arquivos em cache
//  após o service worker ter sido instalado,
//  ou seja, após não acontecer nenhum problema.
self.addEventListener('install', function(e) {
  e.waitUntil(
    caches.open(cacheName)
      .then(function(cache) {
        // adiciona os arquivos em cache
        return cache.addAll(filesToCache);
      })
  );
});

// evento de ATIVAÇÃO do service worker,
//  esse evento é chamado quando seu service worker
//  ja foi instalado previamente, ou seja, não é a 
//  primeira vez que o seu usuário entra em seu site.
self.addEventListener('activate', function(e) {
  e.waitUntil(
    caches.keys()
      .then(function(keyList) {
        return Promise.all(keyList.map(function(key) {
          // para manter nosso cache sempre atualizado e
          //  sem arquivos desnecessários/versões antigas, 
          //  o indicado é verificarmos se no cache do nosso
          //  service worker, as versões que estão lá são
          //  diferentes das atuais, caso seja nós deletamos
          //  a antiga para deixar somente a versão mais atual.
          if (key !== cacheName) {
            return caches.delete(key);
          }
        }));
      })
  );
  // a linha abaixo é para resolver um problema
  //  do service worker não retornar as informações
  //  mais recentes da aplicação no momento que seu 
  //  usuário acessa a página, issso acontece por que
  //  seu service worker ainda não foi ativado, dessa forma
  //  nós fazemos ele ativar mais rápido com as informações
  //  mais novas que ele possuir.
  return self.clients.claim();
});

// evento de FETCHING do service worker,
//  ele verificará quando alguma requisição corresponde
//  a algum arquivo que ja está no cache,
//  caso esteja ele retornará o arquivo do cache,
//  se não estiver, fará a requisição do arquivo
//  e retornará o resultado dessa requisição.
self.addEventListener('fetch', function(e) {
  e.respondWith(
    caches.match(e.request)
      .then(function(response) {
        return response || fetch(e.request);
      })
  );
});

</code></pre>
</div>

### Registrando um Service Worker

Se você tentou botar esse arquivo no seu projeto e não funcionou, realmente só ele não fará nada, o browser nao consegue reconhecê-lo sozinho, nós temos que registrar esse arquivo na API de service workers. 

E fique calmo que é algo bem simples, no seu arquivo principal do app, você passará o diretório do seu arquivo *service-worker.js* para a API.

<div class="code javascript">
  <span class="file-name">app.js</span>
<pre><code>
// importante verificar se o navegador suporta service workers.
if ('serviceWorker' in navigator) {
  navigator.serviceWorker
            // IMPORTANTE: O diretório passado é relativo
            //  a URL, não ao arquivo app.js
            .register('./service-worker.js')
              .then(function() { console.log('Service Worker registrado ! :)'); })
              .catch(function(e) { console.error(e); });
}

</code></pre>
</div>

Pronto, seu app agora carrega caso o usuário esteja offline. Com a <a target="_blank" href="https://developer.mozilla.org/en-US/docs/Web/API/Cache">caches API</a>, você consegue também armazenar algumas requisições caso você queira disponibilizar pro usuário, além da parte visual, informações. O único problema é que infelizmente na maioria dos casos nós temos que mostrar informações atualizadas para o usuário.

### Manifest.json

Outra coisa importante para fazermos um PWA é termos o arquivo manifest.json, ele é responsável pelas configurações como ícone que irá aparecer na homescreen do seu user caso ele adicione seu PWA a tela inicial, cor de fundo do splash screen, cor do tema da aplicação que irá aparecer na barra do android etc.

<div class="code javascript">
  <span class="file-name">manifest.json</span>
<pre><code>
{
  "name": "Nome do meu App",
  "short_name": "NomeCurto",
  "start_url": "/index.html",
  "icons": [{
    "src": "assets/icons/icon-128x128.png",
      "sizes": "128x128",
      "type": "image/png"
    }, {
      "src": "assets/icons/icon-144x144.png",
      "sizes": "144x144",
      "type": "image/png"
    }, {
      "src": "assets/icons/icon-152x152.png",
      "sizes": "152x152",
      "type": "image/png"
    }, {
      "src": "assets/icons/icon-192x192.png",
      "sizes": "192x192",
      "type": "image/png"
    }, {
      "src": "assets/icons/icon-256x256.png",
      "sizes": "256x256",
      "type": "image/png"
    }],
  "display": "standalone",
  "background_color": "#3f3103",
  "theme_color": "#e2a215"
}

</code></pre>
</div>

- **"name"** será o nome apresentado enquanto seu PWA estiver sendo instalado no celular do usuário.
- **"short_name"** será o nome que fica junto do seu ícone na tela inicial do android.
- **"start_url"** será qual a página inicial a ser aberta quando seu usuário entrar no seu PWA pelo botão na tela inicial.
- **"icons"** você definirá seus ícones, o indicado é você ter vários ícones de tamanhos diferentes, assim o melhor será usado em cada tamanho de tela automaticamente. **IMPORTANTE:** para que apareça a caixa de *"adicionar à tela inicial"* você deve passar no minímo algum ícone com tamanho de 144x144.
- **"display"** serve para você escolher se o seu usuário deverá ver a barra de endereços do navegador ou não, caso ele entre em seu PWA pelo botão da tela inicial.
- **"background_color"** é a cor sólida apresentada ao seu usuário enquanto não acontece o primeiro *painting* do navegador, alterando essa configuração seus usuários não irão mais ver uma tela branca enquanto seu site carrega.
- **"theme_color"** é a cor que terá a barra de ferramentas enquanto o usuário estiver em seu PWA.
- **"orientation"** pode ser *landscape* ou *portrait*, caso você deixe seu *manifest.json* sem a opção seu usuário poderá ver sua página nas duas orientações.

Você pode ver todas essas opções em exemplos <a target="_blank" href="https://developers.google.com/web/fundamentals/engage-and-retain/web-app-manifest/" />aqui</a>.

E importante, claro, informar em sua página html a existência do seu arquivo *manifest.json*.

<div class="code hmtl">
  <span class="file-name">index.html</span>
<pre><code>
&lt;!-- só adicionar essa tag em seu head --&gt;
&lt;link rel="manifest" href="/manifest.json"&gt;

</code></pre>
</div>

### E quando os arquivos forem substituídos?

Sim, você talvez tenha feito essa pergunta, e ela faz sentido caso você esteja preocupado com a experiência do seu usuário, basicamente até o momento nós registramos nosso **SW** e ele só será substituído pelo novo quando o usuário der refresh na página, mas *como ele vai saber se existe uma atualização nova nos arquivos?* Aí que entra o [oncontrollerchange](https://developer.mozilla.org/en-US/docs/Web/API/ServiceWorkerContainer/oncontrollerchange), que irá avisar ao nosso browser que existe um novo worker ativo e dessa forma você poderá mostrar isso ao usuário da maneira que julgar melhor. 

<div class="code javascript">
  <span class="file-name">app.js</span>
<pre><code>
if ('serviceWorker' in navigator) {
  navigator.serviceWorker
            .register('./service-worker.js')
              .then(function() { console.log('Service Worker registrado ! :)'); })
              .catch(function(e) { console.error(e); });

  navigator.serviceWorker.oncontrollerchange = function(controllerchangeevent) {
    // aqui dentro podemos disparar algum evento em uma 
    //  store por exemplo para o usuário saber que existe
    //  novas funcionalidades esperando pelo seu refresh
    alert("Atualize sua página para ter acesso a um conteúdo mais novo");  
  }
}

</code></pre>
</div>

Lembrando que esse evento ta com a tag **WD***(Working Draft)* e pode sofrer alterações no futuro, mas não se preocupe pois nada irá quebrar caso você a use.

### Outras coisas importantes

Teoricamente após fazermos isso tudo nós temos um PWA, mas é importante lembrar que outras preocupações devem ser tomadas, caso você utilize o plugin para chrome <a target="_blank" href="https://chrome.google.com/webstore/detail/lighthouse/blipmdconlkpinefehnmjammfjpmpbjk">Lighthouse</a> poderá ver se seu site pode ser considerado um *progressive web app*, essas outras métricas são coisas como *tempo de load*, *tempo até o primeiro paint na tela*, *se é responsivo*, *se funciona com javascript desabilitado* etc. **LEMBRANDO** o mais importante de todos esses é ter a certificação SSL ! Faça seus usuários navegarem de forma segura bro ! :)

### WEBPACK

Para quem usa Webpack venho informar que não será necessário fazer esse arquivo manualmente sempre, utilizem o <a href="https://github.com/goldhand/sw-precache-webpack-plugin" target="_blank">sw-precache plugin</a> e comecem a cachear seu javascript e todo o resto imediatamente. :D

### Conclusão

Tudo o que vimos aqui foi a parte mais complicada em ter um PWA, sites responsivos e *mobile-friendly* ja são uma realidade e eu creio que você já deve estar aplicando a um tempo, e espero de verdade que utilizem o plugin <a target="_blank" href="https://chrome.google.com/webstore/detail/lighthouse/blipmdconlkpinefehnmjammfjpmpbjk">Lighthouse</a> para verificar como está o desempenho do seu site, ele é muito útil e te informa vários aspectos técnicos do seu app.

Sem esquecer de informar, esse post é o resultado da minha presença no <a href="https://events.withgoogle.com/pwa-roadshow-latin-america/" target="_blank">PWA Roadshow</a> da Google Developers, participar desses eventos é algo muito legal e a melhor fonte de aprendizagem sempre, fica aqui a minha **dica**: sempre que puderem participem de eventos da comunidade, vocês só irão ganhar com isso !

![pwa roadshow event](/assets/pwa/pwa.jpeg)

E assim chegamos ao final de mais um post, espero que tenham gostado e se você acha que esqueci de falar algo informe abaixo. Muito obrigado para quem leu até aqui e críticas, dúvidas ou sugestões já sabem onde ir :) *~comentários~*
---
layout: post
title: "Estado global & Estado local"
image: "/assets/redux/cover.jpg"
date:   2017-03-19 08:00:00 -0200
color_template: "react"
tags: Redux React javascript iniciante
resume: "O que deve ser tratado no Redux e o que deve ser tratado no state do componente? iFood como estudo de caso"
topic: react
comments: true
---

### Introdução

Bom dia, boa tarde ou boa noite, sejam bem vindos a mais um post. Hoje falaremos sobre uma dúvida bem comum entre os desenvolvedores iniciantes que acabaram de descobrir sobre arquiteturas **flux** e o famoso **redux**, *"é para substituir todos os estados de todos os componentes pela nossa store global?"*.

Muitas vezes pessoas vêm me perguntar sobre dicas de como utilizar melhor o **redux**, e não tenha vergonha caso você seja uma dessas pessoas que não sabe utilizar a ferramenta da forma correta, como tudo na área de desenvolvimento front-end, o *redux veio no bonde do hype* e todos só sabiam falar que voce **deveria usá-lo**, sem nem mesmo ver quais os requisitos do seu projeto.

Toda essa confusão gerada normalmente cobra um preço caro com atraso na entrega, ou até mesmo no pior do casos cobra performance na sua aplicação.

Tentando te ajudar a entender se você precisa de **redux** ou não, vou começar esse post pensando que você ja conhece a arquitetura [**flux**](https://facebook.github.io/flux/), caso não conheça, não se preocupe que em breve **esse comentário será trocado por um link de um futuro post explicando a mesma**.

### A teoria

Como aprendemos com o **flux**, nosso *"fluxo de dados"* deve ser unidirecional e não bidirecional. Baseado nessa afirmação, a library do React ja foi construida pensando em suportar esse tipo dataflow, quando você declara que um componente possui um estado, na verdade você sinaliza que ele tem uma store, e todas as alterações que você vá realizar nessa árvore de componentes devem ser realizadas na *store* do componente pai, que passará novamente as propriedades para o filho, fechando o *"fluxo"*.

A utilidade do redux está exatamente em quando essa arvore fica cada vez maior e existe a necessidade de mais de um componente ou view/rota acessar a mesma informação, como no caso de um e-commerce que você talvez mostre a quantidade de itens que o usuario tem no carrinho em algum canto da tela, e no container principal mostra pro usuário que esse produto está selecionado, ambos os casos eles precisarão provavelmente acessar alguma estrutura no store global que irá ter todos os produtos que usuário ja selecionou e suas respectivas quantidades.

### Exemplos práticos no iFood

Primeiramente gostaria de avisar que o exemplo é só visual, não identifiquei no iFood nenhuma estrutura *React*, **Redux** ou até mesmo indicando ser **SPA**.

#### Estudo de caso

![Imagem demonstrando o carrinho vazio no site ifood](/assets/redux/carrinho_vazio.png)

Como podem ver na imagem acima, nosso carrinho está vazio, caso naveguemos por outros restaurantes iremos visualizar a mesma imagem em todos, nosso pedido estará vazio.

![Imagem demonstrando carrinho ao adicionarmos um item](/assets/redux/carrinho_com_itens.png)

Ao adicionarmos um item no nosso pedido, nosso carrinho é populado pelos itens e suas respectivas quantidadades, como esses dados serão utilizados através de todas as rotas e views na aplicação, ele deve ser mantido em uma store global, somente dessa forma conseguiriamos ter o resultado abaixo.

![Imagem demonstrando que o carrinho está sendo utilizado em outro restaurante](/assets/redux/carrinho_cheio_outra_janela.png)

As regras de negócio do iFood não permitem realizarmos mais de um pedido em retaurantes diferentes numa mesma sessão, então em todas as rotas que visitamos a aplicação precisa saber que um pedido ja está em aberto e saber quais são os itens que estão nesse pedido, pois o usuário pode querer revisar o que ele pediu em outro restaurante a qualquer momento

![Imagem demonstrando popup do carrinho aberto](/assets/redux/carrinho_aberto.png)

#### Estado local

Em locais como a página do restaurante e o cardárpio oferecido não precisam serem armazenados na store global, afinal essas informações podem ser alteradas a qualquer momento e coisas como valores dos pratos e informações sobre funcionamento devem ter consistência para o usuário toda vez que ele entra na página do restaurante.

![Imagem demonstrando a página do restaurante e listagem do cardápio](/assets/redux/estado_local.png)

Igualmente as avaliações dos usuários, que só serão uteis quando o usuário estiver naquela página.

![Imagem demonstrando a página dos comentários do restaurante](/assets/redux/comentarios.png)

### Conclusão

Saber a hora de utilizar o **redux***(estado global)* ou o *estado local* é muito útil, pois se errarmos na nossa decisão, acabaremos utilizando espaço da memória desnecessário.

Alguns itens realmente serão sempre necessários de estar no estado global, informações do usuário por exemplo sempre precisamos ter em todas as rotas, afinal em todas elas deveremos ter algo informando quem está conectado, ou no caso do ifood a aplicação deverá mostrar a distancia entre o endereço do usuário para o endereço do restaurante, então devemos ter o endereço do usuário com acesso rápido.

O post fica por aqui, agradeço imensamente seu tempo e espero que tenha gostado, se acha que expliquei algo de forma errada ou ficou uma pinta de dúvida, poste ela nos comentários ! além de sugestões e criticas também, caso as tenha ! 

E se realmente você tem um caso que não sabe se precisa usar redux ou não, não tenho problemas em ajudar, só deixar ai em baixo ou me mandar um email ! :)

Obrigado de novo, e até o próximo post ! :D
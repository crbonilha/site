---
title: "#1 Road to GM - Codeforces Round 875 Div 1"
description: "Iniciando minha jornada rumo a Grandmaster"
slug: 1-road-to-gm
date: 2023-06-10 00:00:00+0000
image: "cf-875.png"
categories:
    - Road to GM
---

Há muito tempo eu tenho como objetivo alcançar o título de Grandmaster no Codeforces,
mas eu nunca me esforcei de uma maneira eficiente. Dessa vez eu quero estruturar
meu treino e trabalhar mais especificamente nas minhas fraquezas. Em breve pretendo
escrever um post mais detalhado sobre isso.

Para cada contest que eu participar, vou escrever um post com a minha
opinião sobre os exercícios, tanto aqueles que eu resolvi como aqueles que eu não resolvi.
Para os que eu não resolvi, vou tentar resolvê-los após o contest, e atualizar o post
sobre o que foi necessário aprender para poder resolvê-lo.

Enfim, duas semanas atrás eu participei do
[Codeforces Round 875 - Div 1](https://codeforces.com/contest/1830). Eu resolvi o problema A,
e quase resolvi o problema B (errei por causa de um detalhe besta que vou comentar abaixo).

## Problema A - Copil Copac Draws Trees

Esse problema foi relativamente fácil. Dada uma árvore com uma raíz definida,
onde cada aresta tem uma propriedade _e(i)_ que é usada para calcular o peso dela,
descubra o maior custo entre os caminhos entre a raíz e as folhas.

A propriedade _e(i)_ é igual ao índice da aresta _i_ na lista do Copil.

Seja _p(i)_ o vértice pai do vértice _i_,
e seja _g(i)_ o vértice pai do vértice pai do vértice _i_ (ou seja, _g(i) = p(p(i))_),
o custo de uma aresta {_p\_i_, _i_} é calculado da seguinte maneira:

 * (1) se _p(i)_ é igual à raiz, então o peso da aresta {_p\_i_, _i_} é igual a 0;
 * senão:
    * (2) se a propriedade _e({g\_i, p\_i})_ é menor que a propriedade
      _e({p\_i, i})_, então o peso da aresta {_p\_i_, _i_} é igual a 0;
    * (3) senão o peso é igual a 1;

(1) O motivo é que a raíz é desenhada no Step 0, então no Step 1 a aresta que sai
da raíz já pode ser desenhada.

(2) O motivo é que em uma única passagem do Step 1, ambos os vértices _p\_i_ e
_i_ vão ser desenhados, pois a aresta {_g\_i_, _p\_i_} é desenhada antes da aresta
{_p\_i_, _i_}, logo o custo é 0.

(3) Isso é contrário da condição 2: se o índice da aresta {_g\_i_, _p\_i_} é
maior do que o da aresta {_p\_i_, _i_}, os vértices serão desenhados em duas passagens
diferentes do Step 1, logo o custo é 1.

Dada essa definição dos pesos, basta fazer uma busca em largura ou profundidade na
árvore inteira e computar qual foi o maior custo entre a raís e cada folha.

## Problema B - The BOSS Can Count Pairs

Esse problema também não foi muito difícil, porém um detalhe me passou despercebido.

Logo no começo eu percebi que como _bi_ era no máximo _N_, logo _bi_ + _bj_ <= 2 * N,
e logo só importam os índices i e j tal que _ai_ * _aj_ <= 2 * N, logo _ai_ ou _aj_ <= raiz(2 * N).

Então eu escrevi uma solução baseada nisso, usando um mapa pra checar se determinados itens
já haviam sido processados, e foi aí que eu errei.

Em C++, eu usei a seguinte operação para checar se um item no mapa existia: `m[key] != 0`.
Isso até faz sentido, por que quando eu adicionava um item no mapa ele sempre tinha valor
maior que zero. O problema é que, ao checar se o item existe dessa maneira, o mapa
automaticamente cria esse item caso ele não exista. Logo, toda vez que eu checava,
um novo item era criado, e isso foi sobrecarregando o mapa com muito mais
itens do que existe na entrada do exercício.

Enfim, eu passei uma semana tentando achar estratégias diferentes pra resolver o exercício,
até eu perceber que o erro era esse, mudar a comparação para `m.find(key) != m.end()`,
e ver a minha solução original passar.

## Problema C - Hyperregular Bracket Strings

Eu ainda não resolvi esse exercício, mas é fácil perceber que existe uma solução
trivial usando programação dinâmica que usa O(N^2) de espaço e memória. Talvez
exista um truque pra otimizar isso. Vou continuar pensando.

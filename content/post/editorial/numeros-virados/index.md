---
title: Números Virados - Editorial 
description: "Editorial do exercício que eu escrevi para a Fase regional da Maratona de Programação 2022"
slug: numeros-virados
date: 2023-06-17 00:00:00+0000
draft: true
---

A fase regional da Maratona de Programação 2022 aconteceu no dia 8 de outubro de 2022, onde participaram
556 times de 170 instituições brasileiras. Você pode ler mais detalhes sobre a competição no
[site da maratona](http://maratona.sbc.org.br/hist/2022/primfase22/).

Eu tive o privilégio de escrever um dos problemas, o problema "N - Números Virados". Você pode
[vê-lo no caderno de problemas da competição](http://maratona.sbc.org.br/hist/2022/primfase22/reports/problems/maratona.pdf)
ou [resolvê-lo no site da Beecrowd](https://www.beecrowd.com.br/judge/pt/problems/view/3437).

O meu público alvo são competidores iniciantes, então nesse post eu vou tentar explicar a solução da
maneira mais didática que eu conseguir. Talvez o post fique um pouco extenso, mas eu espero que fique
claro.

## Abstraindo o problema

Geralmente os problemas da maratona costumam colocar uma estória artificial que explica o contexto,
e em seguida descrever o problema a ser resolvido naquele contexto. No caso desse problema, a estória
é até bem direta se comparada com outros problemas da competição: você tem um baralho, e um amigo seu
te explica as regras de um jogo e te desafia a conseguir a maior pontuação possível.

No contexto desse problema, é fácil notar que podemos dividir o problema em duas partes:
escolher K cartas (sempre respeitando a restrição de pegar cartas das pontas), e dentre estas
escolher L cartas (sem restrição).

Usando termos da computação, podemos dizer que a primeira parte envolve escolher um prefixo P
e um sufixo S, tal que |P| + |S| = K (note que |A| representa o tamanho de um conjunto), e a segunda
parte envolve escolher um subconjunto T da união de P e S, tal que |T| = L.

A segunda parte do problema depende da escolha feita na primeira parte do problema.

## Primeira parte - prefixos e sufixos

Primeiro vamos fazer uma observação sobre a ordem de escolhas: mais de uma ordem pode resultar no mesmo
prefixo e sufixo. Essa observação diminui consideralmente a quantidade de possíveis opções.

Por exemplo, vamos supor que N = 4 e K = 2. De acordo com a descrição do problema, devemos escolher
K cartas, respeitando a restrição de pegar cartas das pontas. Então nossas possíveis opções são:
(esquerda, esquerda), (esquerda, direita), (direita, esquerda), e (direita, direita).

Agora note que ambas as opções (esquerda, direita) e (direita, esquerda) resultam em pegar uma carta
de cada lado. Note também que a ordem em que as cartas foram escolhidas é irrelevante para a segunda
parte do problema. Logo, podemos dizer que ambas as opções são equivalentes.

No exemplo descrito, usando-se dessa observação nós reduzimos a quantidade de opções de 4 para 3, o que
pode não parecer muito, mas conforme o valor K cresce, a diferença de opções cresce também.

Veja, se listarmos todas as opções usando a descrição do problema, a cada etapa nós temos duas escolhas
(esquerda ou direita), e vamos repetir isso K vezes, logo a quantidade de opções é igual a 2^K. Porém, se ignorarmos
todas as opções equivalentes, isso seria o equivalente a escolher um par de inteiros p e s, tal que p + s = K,
que representa que iremos pegar p cartas da esquerda e s cartas da direita.
As opções então se resumem a: (p=0, s=K), (p=1, s=K-1), ..., (p=K-1, s=1), e (p=K, s=0). Ou seja, existem
K+1 opções. ERRADO

Agora que listamos todas as possívels opções para a primeira parte do problema, vamos
para a segunda parte.

## Segunda parte - subconjuntos

A segunda parte envolve escolher um subconjunto de tamanho L dentre os K elementos escolhidos
na primeira parte. Isso, por si só, é um problema clássico de combinatória: combinação, dados
K elementos, escolha L. A quantidade total de opções é igual a K! / L! * (K - L)!.

Dado que a quantidade de opções é muito alta, iterar sobre todas elas e escolher a que
resulta na maior pontuação é inviável.

Porém, dado que o objetivo é escolher a maior pontuação, existe uma solução mais simples:
basta ordenarmos os K elementos e escolher os L maiores dentre eles.

Se combinarmos as duas partes, até então nosso algoritmo tem a seguinte complexidade:
O(K) da primeira parte, O(K lg K + L) da segunda parte, ou seja, O(K ^ 2 lg K).
Dado que K <= N <= 10^5, no pior caso esse algoritmo será muito lento e tomará
tempo limite excedido.

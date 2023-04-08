---
title: Contest Bonilha - Editorial
slug: contest-bonilha-editorial
date: 2014-03-03 00:00:00+0000
categories:
    - Editorial
---

Olá maratonistas! Tive a honra de escrever os exercícios para o primeiro contest aberto ao público do URI, entitulado “Contest Bonilha”. Agradeço a oportunidade ao pessoal do portal, em especial ao apoio do Neilor e do Jean, e também ao maratonista Ricardo Oliveira, por revisar todos os exercícios e eventualmente corrigir o enunciado e/ou os arquivos de alguns deles.

Parabéns aos melhores colocados do contest:
- 1⁰ lugar: Marcos Kawakami
- 2⁰ lugar: dilsonguim
- 3⁰ lugar: Hasan0540

Vou escrever um pequeno editorial com algumas dicas de resolução dos exercícios. Aconselho que leia o editorial apenas após tentar exaustivamente resolvê-los, e peço que me corrijam caso eu fale alguma bobagem.


## [Hello Galaxy](https://www.beecrowd.com.br/judge/pt/problems/view/1515)

Como descobrir quando algum planeta enviou a mensagem?
R: Subtraindo o tempo de viagem do ano de chegada.

Portanto o primeiro planeta a enviar é aquele que tem o menor valor em `tempoChegada – tempoViagem`.


## [Competição](https://www.beecrowd.com.br/judge/pt/problems/view/1514)

O enunciado pode parecer confuso, porém a solução para o exercício não esconde nenhum segredo: basta checar cada uma das características e imprimir o resultado.

1 – Ninguém resolveu todos os problemas.
Se houver ao menos uma linha apenas com valores 1, esta características é falsa, pois tal linha corresponde a um competidor que resolveu todos os problemas.

2 – Todo problema foi resolvido por pelo menos uma pessoa.
Se houver ao menos uma coluna apenas com valores 0, esta característica é falsa, pois tal coluna corresponde a um problema que não foi resolvido por ninguém.

Os outros dois são bem semelhantes.


## [Imagem](https://www.beecrowd.com.br/judge/pt/problems/view/1516)

Basta repetir cada linha A/N vezes, e cada coluna B/M vezes. Por exemplo, se N = 3 e A = 12, basta repetir cada linha 4 vezes, pois 3 linhas repetidas 4 vezes é igual a 12 linhas (3 * 4 = 12).

O problema recai então na implementação da repetição. Há diversas maneiras de se implementar isso, então vou deixar o trecho do código sobre como eu implementei: https://github.com/crbonilha/codes/blob/master/imagem.cpp


## [Azulejos](https://www.beecrowd.com.br/judge/pt/problems/view/1512)

O objetivo deste exercício era apresentar o conceito de União de Conjuntos, mais especificamente a teoria da Inclusão-Exclusão.

Vejamos, uma forma rápida de descobrir quantos azulejos serão pintados é a seguinte fórmula:
`total = piso(N/A) + piso(N/B)`.

Mas não é difícil notar que alguns azulejos podem estar sendo contados mais de uma vez, como na imagem abaixo.

TODO: inserir imagem.

```
total = piso(6/2) + piso(6/3)
total = 3 + 2 = 5
```

Porém, podemos contar apenas 4 azulejos pintados.

Aí entra a teoria da Inclusão-Exclusão: temos dois conjuntos, e ao somá-los estamos somando elementos repetidos. Para concertar isso, devemos subtrair a intersecção de tais conjuntos.

Note, um número é múltiplo de A e de B se, e somente se, é múltiplo de MMC(A, B) (google mínimo múltiplo comum).

Solução: `A + B – (A U B)`


## [Maçãs](https://www.beecrowd.com.br/judge/pt/problems/view/1517)

Na transição entre o tempo t e o tempo t+1, você pode fazer duas coisas: se mover ou ficar onde está. O problema está em decidir qual dessas escolhas vai lhe render o maior número possível de maçãs.

Não há saída a não ser testar todas as opções, e nada melhor para fazer isso do que o reaproveitamento de cálculos: Programação Dinâmica.

Se esse conceito é novo para você, pode ser uma boa oportunidade de aprendê-lo. Programação Dinâmica se trata de reaproveitar cálculos que, no decorrer do algoritmo, podem ser executados mais de uma vez.

O primeiro passo é dividir o problema em etapas, ou estados. Imagine que você está na posição (x, y) no tempo t. Este é um estado: (x, y, t).

Uma vez que você descobrir qual a melhor maneira de coletar maçãs a partir deste estado, salve tal solução e a utilize sempre que no decorrer do algoritmo você se encontre no mesmo estado.

A solução é: `Total(x, y, t) = max(Total(novo_x, novo_y, t+1), para todo (novo_x, novo_y) que corresponde a um quadrado ajdacente a (x, y), ou mesmo (x, y))`. Some 1 sempre que houver uma maçã na posição (x, y), no tempo t.

Para mais detalhes pesquisem por Programação Dinâmica.

EDIT:

A solução que eu citei foi a que eu imaginei quando escrevi o exercício, mas conversando com alguns maratonistas descobri uma segunda forma de resolver tal exercício, com uma complexidade menor.

Seja x, y e t informações correspondentes a uma maçã em particular, e nx, ny e nt as informações correspondentes a uma segunda maçã em particular, é possível chegar da posição (x, y) até a posição (nx, ny) em nt-t segundos? (Pense numa fórmula matemática).
Se sim, podemos capturar ambas as maçãs. Se não, podemos capturar uma das duas, mas não ambas.
Utilizando-se Programação Dinâmica, podemos reaproveitar alguns cálculos e testar todas as possibilidades.


## [Cavalo](https://www.beecrowd.com.br/judge/pt/problems/view/1513)

Este exercício se divide em dois problemas: descobrir o caminho mínimo entre cada uma das peças, considerando a particularidade do salto do cavalo; e descobrir qual a melhor sequência de captura de peões a ser seguida.

O caminho mínimo pode ser resolvido utilizando uma Busca em Largura (BFS) a partir de cada uma das peças até as demais. Ao final de tais cálculos, teremos um grafo com K+1 vértices, todos interligados com arestas que correspondem ao caminho mínimo entre cada uma das peças.

A segunda parte envolve, como no exercício anterior, a checagem de todos os possíveis de caminhos.

Podemos começar capturando qualquer um dos K peões. Em seguida, poderemos capturar qualquer um dos K-1 peões restantes. Em seguida, K-2 peões. Até que tenhamos capturados todos os peões e precisemos retornar à posição inicial.
Ao final teremos a seguinte complexidade: K! (K * K-1 * … * 2 * 1).
Como o maior valor de K é 15, temos uma complexidade de 15!, que é um valor muito alto.

Para amenizar isso podemos utilizar uma técnica conhecida como bitmask. Trata-se de uma forma de traduzir quais peões você já capturou em um número inteiro, ou seja, um estado.
Tal estado pode ser utilizado para reaproveitar cálculos, e temos então nossa complexidade reduzida.

Para mais detalhes pesquisem por Bitmask, Programação Dinâmica e Ciclo Hamiltoniano.


## [Tartarugas](https://www.beecrowd.com.br/judge/pt/problems/view/1518)

Eis uma variação do exercício anterior. Temos que capturar desta vez tartarugas, porém as mesmas se movem, e é quase impossível reaproveitar cálculos aqui. Por sorte o número de tartarugas é baixo, e 3! não é uma complexidade assustadora.

Veja, podemos capturar as tartarugas nas seguintes ordens: 1->2->3; 1->3->2; 2->1->3; 2->3->1; 3->1->2; 3->2->1. Totalizando 6 ordens (3!).

O desafio está em descobrir quantos segundos leva-se para alcançar um alvo em movimento. Há duas soluções: simular a corrida, e ver quantos segundos leva; chutar valores e utilizar o conceito de busca binária para adicionar precisão à escolha.

Quanto à segunda solução citada, eis uma explicação dada pelo maratonista Ricardo Oliveira:

```
Seja t o menor tempo necessário para se alcançar uma tartaruga inicialmente em
(Tx, Ty) (e, após t segundos, em (Tx+t, Ty) (ou (Tx,Ty+t), de acordo com a
direção)) a partir do ponto (Rx, Ry).
Seja t' > t. É válido notar que, como Rafael pode ser mais rápido que a
tartaruga, Rafael pode chegar em (Tx+t', Ty) *antes* da tartaruga e esperar por
ela neste ponto. Além disso, se t' < t, a tartaruga irá chegar em (Tx+t', Ty)
antes de Rafael, e portanto não será alcançada.
Assim, o valor de t é o menor valor de t' tal que Rafael pode chegar em (Tx+t',
Ty) antes (ou no mesmo tempo) da tartaruga. Logo, é possível encontrar tal valor
com uma busca binária: para cada número t' analisado, verifica-se se Rafael pode
chegar antes ou ao mesmo tempo em (Tx+t',Ty) que a tartaruga, e o intervalo é
atualizado de acordo.
Obviamente a tartaruga leva t' segundos para ir de (Tx,Ty) para (Tx+t', Ty).
O problema se limita então a definir quanto tempo Rafael leva para ir de
(Rx,Ry) para (Tx+t', Ty). Uma análise dos movimentos de Rafael, e principalmente
o fato de ele leva 2 segundos para ir de (Rx,Ry) para, por exemplo, (Rx+2,Ry+2),
*independentemente do caminho que ele seguir*, nos permite concluir que Rafael
pode sempre seguir um caminho guloso, que leva min(dx, dy) + teto(abs(dx-dy)/2)
segundos, onde dx = (Tx+t' - Rx) e dy = (Ty - Ry).
```


## Conclusão

Então é isso. Se você teve uma solução diferente e quiser compartilhar, ou ainda estiver com alguma dúvida, deixe aqui embaixo seu comentário, ou me envie um e-mail.

Espero que tenham gostado do contest. Até mais.

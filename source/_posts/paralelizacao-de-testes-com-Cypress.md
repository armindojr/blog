---
title: Paralelização de testes com Cypress
date: 2020-06-03 12:00:00
tags: [cypress, parallel, automation]
---
**Qual o motivo de paralelizar testes ?**
Recentemente fiz parte de um projeto onde tínhamos diversas pessoas que criavam e automatizavam cenários de testes no mesmo repositório para testes do tipo regressivo, aos poucos percebemos que nosso repositório de testes aumentou exponencialmente seu tempo de execução dentro do CI, e com isso estudamos diversas formas de melhorar esse tempo. Com todo esse background, decidimos optar por incluir a execução de testes de forma paralela.
<!-- more -->

**Como o Cypress funciona com a paralelização**
Por padrão, o Cypress possui uma integração com seu próprio dashboard para a realização desses tipos de testes, o chamado Cypress dashboard. Esse dashboard possui diversos benefícios tais como:
- Veja quantos testes falharam ou passaram
- Obtenha o rastreamento completo dos testes que possuem falha.
- Veja as capturas de tela tiradas com falha no teste.
- Assista a um vídeo de toda a sua execução de teste ou no ponto de falha.
- Gerencie quem tem acesso aos seus reports.

Porém esse método traria os seguintes pontos negativos:
- Extremamente caro para grandes projetos
- Pouco controle sobre como a execução será feita, já que a dashboard tem um loadbalancer próprio

Também chegamos em um pacote chamado [sorry-cypress](https://github.com/agoldis/sorry-cypress) que serve pra substituir a dashboard do próprio Cypress, porém com essa implementação perderíamos o report unificado do mochawesome e teríamos uma dificuldade muito grande de implementar dentro do nosso CI.

Por fim, decidimos em conjunto que iriamos tentar achar uma solução onde executaríamos o Cypress paralelamente dentro do próprio Jenkins utilizando apenas a sintaxe dentro do próprio jenkinsfile em conjunto com a visualização do plugin **BlueOcean**.

**Mas porque usar o Blue Ocean ?**
Este é um plugin criado para o Jenkins com o intuito de melhorar e facilitar as visualizações de toda interface dos Jobs, usamos ele pela forma como ele simplifica a execução do Job e por sua interface deixar o Jenkins mais clean, mostrando exatamente como a execução paralela está sendo feita e o console exato de cada POD. Diferente do console padrão do Jenkins que somente mostra os logs sequencialmente, o Blue Ocean quebra exatamente como os PODs estão configurados.

**Implementando a paralelização diretamente no CI**
Como já foi comentado, a implementação dessa funcionalidade demandou apenas estudar a sintaxe de estágios paralelos nativo do Jenkins e um código que separa as specs de acordo com a quantidade de PODs à ser executado.

**Jenkinsfile**

//imagem 1

Para utilizar a execução paralela do Jenkins é bem simples, quebramos as execuções dentro de contêineres diferentes e colocamos o comando para execução do Cypress dentro bloco de cada contêiner como na imagem acima.

//imagem 2

Nessa parte do código devemos realizar a configuração do report Mocha Awesome onde toda a magia acontece, aqui você configura toda as variáveis e caminhos para que o report seja gerado corretamente e não seja perdido nenhum step.

Colocamos abaixo algumas imagens para exemplificar a forma que o Jenkins exibe estes testes dentro de um Job.

**Pagina inicial do Job**

//imagem 3

**Pagina de apresentação dos estágios pelo Blue Ocean**

//imagem 4

**Visualização do console de execução no POD A**

//imagem 5

**Visualização do console de execução no POD B**

//imagem 6

**Como o report fica com a execução de forma paralela**

//imagem 7

**Resultados da implementação**
Abaixo está uma imagem onde apresentamos os resultados da nossa implementação de paralelização dentro do próprio Jenkins. Claramente percebemos que a execução de testes nesse modo demonstra um ganho de quase a metade do tempo nas execuções.

//imagem 8

Caso queiram estudar a sintaxe de estágios em paralelo no Jenkins segue um exemplo interessante:

- [Parallel stages](https://www.jenkins.io/blog/2017/09/25/declarative-1/)

Segue o link do projeto de exemplo de como essa funcionalidade foi implementada:

- [Github](https://github.com/armindojr/parallel-cypress-example)

Este artigo foi feito em conjunto com um amigo de equipe que ajudou a implementar essa funcionalidade. Obrigado por participar disso [Danilo Juhas](https://medium.com/@danilojuhas) :)

Valeu pessoal, até a próxima!
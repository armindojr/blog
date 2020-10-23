---
title: O paradoxo do pesticida em automação de testes
date: 2019-02-11 12:00:00
tags: [automacao, processo]
---

De acordo com o syllabus do CTFL, certificação que garante conhecimentos básicos da área de qualidade, um dos sete princípios do teste é o Paradoxo do pesticida. Mas afinal, o que é este paradoxo e para que serve ?

<!-- more -->

Primeiro vamos começar com a explicação do por quê esse nome e da história por trás.
De acordo com a teoria da evolução, as espécies atuais descendem de outras espécies que sofreram modificações ao longo do tempo e passaram essas características para seus descendentes e essas características são relacionadas ao processo de seleção natural, na qual apenas os que possuem “as melhores características” deixam mais descendentes.
Certo, mas qual a relação disso com o paradoxo do pesticida ?

De forma resumida, foi realizado um estudo com um grupo de baratas do tipo Blattella germanica que mostrou que ao longo do tempo um determinado grupo de baratas apresentou um nível de resistência maior aos inseticidas. Assim, esse grupo com genética superior deixariam seus descendentes cada vez mais resistentes aos inseticidas comuns do mercado.
A ligação dessa história por trás do estudo com baratas e com nossa área de qualidade é que quanto mais aplicamos os mesmos casos de teste no mesmo sistema a ser testado, menos os bugs vão aparecer naquela determinada área testada. Porém outros bugs podem ser criados em outras áreas do sistema que não são abordadas pelos casos de teste comuns criado pela equipe de qualidade.

De acordo com o engenheiro de software Boris Beizer, que foi o primeiro a nomear e apresentar a definição do paradoxo do pesticida, todo método de prevenir ou encontrar bugs deixam um resíduo de problemas mais sutis nos quais os métodos em questão são ineficazes. Ele complementa também que, a complexidade do software (e os bugs desse) cresce até o limite da nossa habilidade de gerenciar essa complexidade.

_As suítes de testes se desgastam_

Pode ocorrer de uma suíte de teste que por um momento se mostrou bastante eficaz, se desgastar pois os programadores e os designers modificam seus hábitos na tentativa de reduzir a incidência de erros. Se você não organizar suas estatísticas de bugs em uma classificação racional, você pode não saber o quão efetivo seu teste foi e nem saber o quão desgastada sua suíte de teste está.

Para efeitos de visualização do tema em questão, digamos que estamos testando um sistema onde nosso plano de teste aborda que a principal funcionalidade a ser testada seria a busca de um produto em um e-commerce e um dos casos de teste espera que o resultado de buscar o produto deveria ser a página de busca contendo o produto que foi buscado. Realizamos o nosso caso de teste e abrimos um bug indicando que a busca não mostrava o produto esperado. Quando os desenvolvedores finalizarem a correção do bug e constatarmos que a tarefa foi realmente finalizada corretamente, nossa suíte de testes que antes tinha um caso de teste que cobria um resultado específico se torna obsoleta por sempre verificar algo que já está correto. Assim, devemos incrementar nossos casos de teste para que atenda outros cenários que antes não eram esperados.

_Como evitar então que o paradoxo aconteça ?_

Podemos seguir a padronização internacional de criação de casos de teste (IEEE 829) durante o ciclo de desenvolvimento e no final da execução dos testes, reavaliar os casos e o desgaste da suíte criada mantendo assim um padrão onde sempre verificamos e atualizamos os testes a serem realizados mesmo que a suíte de teste antiga passe sem problemas.

{% img https://arquivo.devmedia.com.br/REVISTAS/java/imagens/101/08_Processo_Teste_Software/image001.png '"Figura 1. Ciclo de teste" "Figura 1. Ciclo de teste"' %}


**Referências**

CTFL, Syllabus 2018
[Cola da web, Adaptação dos seres vivos](https://www.coladaweb.com/biologia/evolucao/adaptacao)
[Fapesp pesquisas, Baratas resistentes aos inseticidas 2002](http://revistapesquisa.fapesp.br/2002/01/01/baratas-resistentes-aos-inseticidas/)
[Boris Beizer, Software testing techniques 2d. Ed. — Infinite undo! blog 2002](https://infiniteundo.com/post/87158389858/pesticide-paradox)
[University of Otago, IEEE 829–1998 Summary](http://www.cs.otago.ac.nz/cosc345/lecs/lec22/testplan.htm)
[Sergio Barriviera, Padrão para documentação de teste de software 2012](https://www.devmedia.com.br/padrao-para-documentacao-de-teste-de-software/26534)
[Devmedia, processo de teste de software 2012](https://www.devmedia.com.br/processo-de-teste-de-software/23795)
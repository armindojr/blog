---
title: Como aprendi novas skills modificando o FakerJS
date: 2020-05-26 12:00:00
tags: [pure, faker, geracao-dados, mock, fake, automacao]
---
**O que é o pure-gen**
O pure-gen é basicamente um gerador de dados falsos para preenchimento de formulários de teste baseados em uma localidade do mundo. Assim podendo gerar nomes, e-mails, documentos e outras funcionalidades que são únicas de cada país.

<!-- more -->

**A base do projeto**
Para criar o **pure** eu usei como base um projeto muito conhecido pelo pessoal de QA, o **Faker.js**. O projeto já está bem consolidado como um dos mais famosos gerador de dados de JS.

Porém no cenário que vi dentro da equipe que trabalhei, algumas coisas no projeto estavam há muito tempo sem serem atualizadas ou melhoradas, principalmente a parte de localização em PT_BR que possuía algumas falhas. Levando isso em consideração eu pensei em apenas atualizar a localização e criar uma PR para o projeto original. Porém quando fui visitar a pagina de PR’s pendentes no **Faker.js** percebi que tinham muitas alterações pendentes importantes e que o dono original não implementava.

O criador original tem seus motivos para manter o projeto no estado atual, entretanto eu decidi criar o meu próprio baseado neste e implementar as alterações que minha equipe precisava sem depender da boa vontade de outras pessoas e ainda de quebra adquirir um conhecimento diferenciado sobre diversas coisas, nada melhor que um DIY.

**Mudanças que fiz no projeto**
Dentre as mudanças que considero importantes feitas no projeto são:
1. Implementei manualmente no meu código muitos PR’s que estavam pendentes do projeto original
2. Adicionei alguns métodos novos para geração de dados
3. Arrumei bugs diversos como : memory leak, criação de ambiente baseado em uma SEED por array, métodos que retornavam undefined e diversas outras coisas
4. Modifiquei o back-end de geração de RNG, que antes usava um PRNG do tipo mersenne-twister e agora usa um PRNG baseado em lfib que demonstrou ter um desempenho incrivelmente melhor quando usado em larga escala
5. Implementei a utilização do husky e do eslint para garantir a qualidade do código entregue
6. Melhorei a cobertura de código nos testes unitários e refatorei todos os testes para executarem mais rapidamente no CI tendo a mesma cobertura.
7. Implementei uma compressão de dados estáticos em GZIP para diminuir o tempo de instalação e o volume de dados do projeto, tendo impacto 0 na funcionalidade
8. Melhorias gerais no código, removendo blocos comentados e arrumando alguns TODO.

Essas são algumas das principais mudanças que fiz, caso queira entender melhor o que foi alterado, como foi feito e ver todas as mudanças realizadas é só acessar o [changelog](https://github.com/armindojr/pure-gen/blob/master/CHANGELOG.md).

**O que aprendi modificando esse projeto**
Parece meio ambicioso modificar um projeto desse nível e achar que tem algo a incrementar … mas pude tirar proveito para aprender os seguintes itens criando meu próprio gerador de dados:

- Como funcionam testes unitários e para que servem
Execução de testes em um ambiente CI para demonstrar falhas
- Como funcionam os relatórios de cobertura de código
Refatoração e melhoria de um código já existente
- Como funcionam os geradores de números aleatórios
- Compressão de dados estáticos
- Como criar meu próprio projeto no NPM
- Como criar uma documentação que possa demonstrar o funcionamento do seu projeto

Se posso tirar uma lição disso é que se tem algo que você vê na comunidade que pode melhorar, abrace o projeto, se possível chame alguns amigos e se desafiem a melhorar, tive uma experiência muito boa e um dos melhores aprendizados, inclusive, para quem quiser conferir tudo que escrevi na prática ou ainda contribuir para o projeto, seguem os links:

[Github](https://github.com/armindojr/pure-gen)
[NPM](https://www.npmjs.com/package/pure-gen)

Valeu pessoal, até a próxima!
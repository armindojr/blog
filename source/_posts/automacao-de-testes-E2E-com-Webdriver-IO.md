---
title: Automação de testes E2E com Webdriver.IO
date: 2019-05-10 12:00:00
tags: [automacao, wdio, e2e]
---
**Objetivos deste guia**
Este guia não abordará métodos de como deixar o código limpo, organização da suíte de testes e utilização de serviços externos ao core que ajudam nos testes. O principal objetivo é mostrar os primeiros passos com a automação de testes E2E com o webdriver.io.

<!-- more -->

**Por trás da framework**
O webdriver.io hoje em dia possui como mantenedor principal o Christian Bromann, Desenvolvedor/QA que participa do time do Saucelabs. O projeto do Webdriverio está também inserido no ecossistema projetos da JS Foundation, nos quais também estão inseridos projetos de peso do Javascript tais como : **Appium, ESlint, Grunt, jQuery, Mocha** e outros.

**Por quê utilizar o webdriver.io ?**
Esse é sempre um questionamento que é feito ao aprender algo novo. Cada framework de automação de testes E2E tem seus prós e contras. Não será realizada a comparação entre as frameworks, nessa guia serão apontados alguns pontos positivos na utilização. Estes são :
- Comunidade ativa :
A comunidade em volta do webdriver.io sempre está ativa, implementando e desenvolvendo funcionalidades novas para serem inseridas na framework.
- Facilidade na configuração inicial :
O webdriver.io dispõe de uma interface de configuração por terminal (CLI) que ajuda a realizar a configuração inicial do projeto. Criando assim um arquivo base que possui itens configuráveis, este arquivo que será executado para a inicialização dos testes.
- Debug em realtime e REPL :
Um ponto considerado positivo desta framework em relação às outras, é o debug intuitivo que pode utilizar somente um breakpoint no próprio código. Além disso é possível contar com o REPL integrado ao pacote que realiza a mesma função do debug, mas sem precisar utilizar breakpoint. Desta forma, é possível executar sem a necessidade de escrever um arquivo inteiro para o teste.
- Compatibilidade :
O webdriver.io é extremamente compatível com diversas funcionalidades úteis na execução e criação de testes E2E, principalmente se o teste for realizado em um ambiente na nuvem (browserstack / saucelabs)
- Documentação de fácil legibilidade :
A documentação do webdriver.io é de fácil entendimento e legibilidade pois possui exemplos e descrição de forma sucinta.
- Seletores específicos para testes de apps em react :
Recentemente foi implementado uma funcionalidade onde é possível utilizar alguns seletores específicos de elementos que não são comuns em html básico e que estão presentes em projetos com react.

Enfim, mãos na massa.

Será utilizado como base o teste da funcionalidade em um site de busca, onde esse site possui algumas peculiaridades. Deverão ser feitas validações para confirmar determinadas ações e utilizar funções de espera para que algo aconteça na página.

**Links úteis**
[Aprenda por definitivo a usar CSS Selector](https://medium.com/automa%C3%A7%C3%A3o-com-batista/aprenda-por-definitivo-a-usar-css-selector-adeus-xpath-1f3956763c2)
[WebdriverIO Website](https://webdriver.io/)

**Configuração inicial**
O que é necessário ter instalado :
- Ter a versão mais recente do Node.JS, no guia foi utilizada a versão 10.15.3
- Ter o JAVA instalado e configurado, no guia foi utilizado o openJDK 11.0.2

O primeiro passo é criar uma pasta para colocar seu projeto e a abrir no seu editor de código de sua preferência. Durante esse guia será utilizado o **Visual Studio Code**

{% img https://miro.medium.com/max/875/1*YycTbehHy8YINMpRoCsFvA.png %}

Será utilizado o terminal que é possível abrir no próprio VSCode.

{% img https://miro.medium.com/max/875/1*5z2QZ-tKK4ywWnTFghywlA.png %}

Em seguida, será criado o `package.json` onde vai ficar armazenado todos os pacotes deste projeto. Use o comando no terminal :

```
npm init -y
```

{% img https://miro.medium.com/max/875/1*an_Hz6XqIi4YZ2Za5dbPLA.png %}

Agora, serão instalados os pacotes e armazenados dentro do `package.json` para a utilização posterior em outras máquinas. Instale o Webdriver.IO e o CLI dele com o comando no terminal :

```
npm i —-save-dev webdriverio @wdio/cli
```

{% img https://miro.medium.com/max/875/1*X6c5Qh1Z-1O9idRC8vsOqw.png %}

Inicialize o projeto na pasta raiz que foi aberta no editor de texto com o comando no terminal :

```
.\node_modules\.bin\wdio config
```

{% img https://miro.medium.com/max/875/1*MXsO7bA4Ofmaf1tzLhrcRw.gif %}

As seguintes perguntas vão aparecer no terminal :
> P: Where should your tests be launched? (Onde os testes vão ser executados ?)
R: local
P: Shall I install the runner plugin for you? (Gostaria de instalar o plugin que executa os testes ?)
R: Yes (Deixe a resposta como Sim)
P: Where is your automation backend located? (Onde seu backend de automação vai estar ?)
R: On my local machine
P: Which framework do you want to use? (Qual a framework semântica à ser utilizada ?)
R: jasmine
P: Shall I install the framework adapter for you? (Gostaria de instalar o adaptador da framework ?)
R: Yes (Deixe a resposta como Sim)
P: Do you want to run WebdriverIO commands synchronous or asynchronous? (Você gostaria de executar os comandos de forma síncrona ou assíncrona ?)
R: sync (Deixe a resposta como síncrona)
P: Where are your test specs located? (Onde está localizada sua pasta de cenários ?)
R: ./spec/*.js
P: Which reporter do you want to use?** (Qual o método de exibição da execução você quer usar ?)
R: spec
P: Shall I install the reporter library for you? (Gostaria de instalar a biblioteca da exibição escolhida ?)
R: Yes (Deixe a resposta como Sim)
P: Do you want to add a service to your test setup?** (Gostaria de adicionar um serviço à sua configuração ?)
R: selenium-standalone
P: Level of logging verbosity (Nível de log no modo verboso)
R: silent (Nenhum warning será apresentado no console durante a execução)
P: What is the base url? (Qual a url base da aplicação a ser testada ?)
R: http://localhost

* As perguntas podem ser respondidas navegando com as setas e confirmar com o enter do teclado

** A pergunta que possui múltiplas escolhas a opção deve ser primeiro marcada com o espaço do teclado para só depois apertarmos enter.

Após essa série de questionamentos sobre a configuração é criado o seguinte arquivo :

{% img https://miro.medium.com/max/875/1*0HzXbKQmhohLmDc5O8s05g.png %}

Agora crie a pasta `spec/`

{% img https://miro.medium.com/max/875/1*0HzXbKQmhohLmDc5O8s05g.png %}

Dentro dessa pasta irão ficar todas as suítes de testes.

Crie e abra um arquivo com o nome `home.js` dentro da pasta `spec/`.

{% img https://miro.medium.com/max/875/1*3trq-RoTWz3rH7SR4OltRw.png %}

Agora descreva os testes usando a sintaxe do Jasmine (Para saber mais basta entrar na documentação oficial) conforme mostra o guia abaixo.

Resumidamente : O describe serve como separação de cada cenário do teste e o it é uma ação específica

Descreva primeiro o cenário onde irá acessar a página inicial.

{% img https://miro.medium.com/max/875/1*K33OJX55z_G9tM6TYGpj3w.png %}

Depois de escrito, execute o teste no terminal, utilize o comando abaixo :

```
.\node_modules\.bin\wdio .\wdio.conf.js
```

Assim o browser será aberto (no caso o firefox pois ainda não foi configurado nenhum outro).

{% img https://miro.medium.com/max/875/1*GISyFB9g9Jc1Wxi2VV88Ng.png %}

“Poxa, mas eu queria rodar meus testes no chrome, como faço ?”

Simples, é só alterar o `wdio.conf.js` com as seguintes opções :

{% img https://miro.medium.com/max/875/1*ZLwdVuBaWiAKN3tfFPZn7A.png %}

“Mas agora o chrome abre, porém não é em tela cheia …”

Para isso, é necessário passar os seguintes argumentos para o webdriver :

{% img https://miro.medium.com/max/875/1*LloAMm7hJYfKAK4wzVBOlA.png %}

Para incrementar um pouco o teste … Crie um cenário onde é realizada uma busca, clique em um resultado e verifique o carregamento.

Antes de dar forma ao teste, é recomendado a leitura de como criar seletores em css para facilitar a escolha do elemento para o programa realizar alguma ação. No início do guia existem links úteis que irão auxiliar no entendimento do assunto.

Assim, incremente os passos do cenário :

{% img https://miro.medium.com/max/875/1*Y8R2J76GzkpratwKs0_NFg.png %}

Os lugares onde ficaram espaços entre as funções, coloque as ações em si. Elas são compostas da seguinte forma :

{% img https://miro.medium.com/max/434/0*oF46ZZ51JFx9nNlL %}

{% img https://miro.medium.com/max/875/1*MVuRKlwZZVVpx7AiZclw6g.png %}

Perceba que no teste implementado existem funções além das que foram explicadas anteriormente :

{% img vertical-align: middle; https://miro.medium.com/max/321/0*Va670NMcPlyEhgM3 %}

Esse parece o seletor já utilizado, porém está retornando um Array com os itens da página, o índice mostra qual item desse Array será feita a interação e as ações são as mesmas de um seletor comum.

{% img https://miro.medium.com/max/189/0*cHcG1yjFzYXg_0WO %}

O browser é um objeto que possui ações específicas baseadas no próprio navegador, na imagem acima é retornado a title da página acessada.

{% img https://miro.medium.com/max/214/0*m8VAcIyqDRuU-atQ %}

Funcionalidade do framework semântico (jasmine), o expect serve para realizar uma verificação que um determinado item possui algo ou contém algo.

Executando o script novamente, todos os resultados são exibidos no terminal indicando o que foi executado e o tempo de execução.

{% img https://miro.medium.com/max/875/1*u0hWIl2cNU8FUdsz0O_pwA.png %}

**Considerações finais**
O guia aborda apenas tópicos básicos para a primeira implementação de uma automação em JS com o Webdriver.io, é recomendado que todos os leitores se aprofundem nas funcionalidades extras que a ferramenta possui, tais como : **execução de tasks com o grunt, relatórios de execução com melhor apresentação do allure, utilização das ultimas implementações do ECMAscript com o babel, testes de regressão visual** e outras funcionalidades.

Para quem conhece sobre o protractor e chegou até o final sem conseguir diferenciar os dois, lembre-se : o protractor foi criado pensando em testes de sites em angular, os desenvolvedores criaram uma flag pra utilizar sites que não foram desenvolvidos em angular, caso a aplicação em teste redirecionar para outra pagina. Assim a intenção da flag era apenas ser utilizada nesse caso, porém muitos tutoriais indicam que o QA habilite isso de forma permanente. Não é considerado um erro, porém a ferramenta não foi criada pra esse propósito, já que até a descrição no site do protractor é : “Protractor is an end-to-end test framework for Angular and AngularJS applications”.

Existe uma grande diversidade de frameworks para testes E2E, porém o ideal é sempre avaliar e escolher a que melhor atende seu time e sua organização.

[O exercício vai ficar no bitbucket](https://bitbucket.org/armindojr/exercicio-wdio)

Valeu pessoal, até a próxima!
# Capítulo 01

## Conhecendo o instrutor e sabendo mais sobre o curso

### **Sobre o instrutor**

Olá, meu nome é Thiago Brasil e na internet eu sou o @tchaguitos. Cursei Ciência da Computação e sou desenvolvedor web há aproximadamente 4 anos. Nesse tempo trabalhei principalmente com as linguagens Python e Javascript e me aventurei em vários projetos utilizando Django, NodeJS, ReactJS e mais algumas outras tecnologias da web. Além disso, trabalhei também com análise e visualização de dados geográficos.

Se você quiser ver alguns dos meus códigos, dá uma passadinha aqui no meu [Github](https://github.com/tchaguitos/)

### **Sobre o curso**

Com o curso "Django Framework na prática" você vai poder iniciar sua carreira como desenvolvedor web utilizando a linguagem Python e aprender a desenvolver aplicações web seguras e escaláveis numa velocidade incrível. O objetivo do curso é explorar as principais funcionalidades do framework por meio da construção de um projeto real e aprender ao longo de seu desenvolvimento o que são as ferramentas que o framework nos dá e como utilizá-las. 

Nesse curso a gente vai construir junto uma aplicação web do zero com Python que consiste num sistema de controle de visitantes para condomínio. A aplicação deverá ser capaz de possibilitar o registro e administração de visitantes e ainda contar com funcionalidades como visualização de informações de visitantes, autorização de entrada e finalização de visitas.

Depois desse curso você estará apto para desenvolver aplicações web robustas utilizando Django framework tendo visão ampla das boas práticas e principais funcionalidades que o framework nos permite utilizar.

### O que iremos construir?

A partir da necessidade de informatizar o processo de registro de visitantes, um famoso condomínio entrou em contato conosco para desenvolver um sistema web que pudesse suprir essa demanda. Ao encontrar problemas na confiabilidade das informações e até mesmo para melhorar o fluxo de trabalho dos porteiros, o condomínio resolveu buscar ajuda.

Nosso sistema web deverá ser capaz de possibilitar o registro de visitantes, exibir os últimos visitantes registrados, exibir informações detalhadas de cada visitante e ainda autorizar e finalizar suas visitas, registrando informações específicas para cada etapa do processo. É interessante lembrar que existem algumas regras que os porteiros seguem à risca para manter a segurança dos moradores e nosso sistema deverá obedecer a essas regras.

Além destas informações, nosso sistema web deverá exibir um resumo das informações como o número de visitantes aguardando autorização, o número de visitantes realizando visitas no condomínio, o número de visitas finalizadas e ainda o número de registros realizados no mês.

## A linguagem de programação Python

Python é uma linguagem de programação de código aberto poderosa, amigável e bastante popular. Com ela é possível desenvolver sistemas web, trabalhar com análise de dados, inteligência artificial e machine learning, nomes muito falados ultimamente, além de desenvolver aplicativos móveis e aplicações para desktop.

A linguagem foi criada por Guido van Rossum em 1991 com o objetivo de ser uma linguagem produtiva e legível, de modo que fosse fácil para humanos trabalharem em seus códigos, tornando menos custoso o processo de desenvolvimento e manutenção de sistemas.

Além da grande variedade de ferramentas que a própria linguagem disponibiliza, Python tem se tornado cada vez mais comum devido a sua utilização em áreas como Data Science, Machine Learning e Inteligência Artificial e, com isso, ganhado diversas bibliotecas e módulos que facilitam o dia a dia do desenvolvedor.

Por último e não menos importante: Python tem caminhado rumo ao status de linguagem de programação mais popular do mundo!

## O que é Django Framework?

Django é um framework para construção de aplicações web escrito em Python gratuito e de código aberto. Um framework tem o objetivo de agrupar funcionalidades que são utilizadas com certa frequência na construção de aplicações exatamente com o intuito de economizar tempo dos desenvolvedores.

O Django, por sua vez, foi construído por desenvolvedores experientes e projetado pra resolver grande parte dos problemas existentes no desenvolvimento web. Sendo assim, a gente pode se concentrar em escrever a nossa aplicação sem a necessidade de ficar reinventando a roda.

Dentre as empresas que utilizam o Django no mundo, temos exemplos grandes como Instagram, Pinterest e a Mozilla. Aqui no Brasil existem diversas empresas que utilizam como a Olist, a Magalu e o pessoal da Globo.com.

## Instalando e configurando o ambiente para iniciar o projeto

Antes de começar a colocar a mão na massa, a gente precisa preparar o nosso ambiente de desenvolvimento. Esse nosso ambiente é bem simples e tem como dependência apenas quatro itens. São eles:

* Python 3.8
* Pip
* Virtualenv
* Django framework

{% hint style="info" %}
Todos os comandos aqui utilizados são compatíveis com distribuições Linux baseadas em Ubuntu \(Debian\) e as aulas foram gravadas utilizando o Ubuntu 20.04
{% endhint %}

### **Python 3.8**

As versões mais recentes das distribuições linux baseadas no Ubuntu \(Debian\) já vêm com a versão 3.8.2 do Python instalada, mas se este não é o seu caso, basta seguir os comandos abaixo.

Antes de instalar o Python de fato, precisamos instalar alguns pacotes que vão nos ajudar com esta tarefa. Primeiro, vamos atualizar nossos pacotes e instalar algumas dependências:

```bash
$ sudo apt update
$ sudo apt install software-properties-common
```

Com isso feito, podemos agora adicionar o repositório PPA que será utilizado como fonte para instalarmos o Python3.8:

```bash
$ sudo add-apt-repository ppa:deadsnakes/ppa
```

O terminal vai pedir para que o você aperte `enter` para confirmar a ação. Faça isso para adicionar o repositório à sua lista e utilize os comandos abaixo para novamente atualizar os pacotes existentes e, finalmente, instalar a versão 3.8 do Python:

```bash
$ sudo apt update
$ sudo apt install python3.8
```

Vamos utilizar esta versão pois é a versão mais recente e estável que temos da linguagem. O Django encoraja a utilização de versões mais recentes da linguagem e ainda oferece suporte oficial sempre para as últimas versões disponíveis. Como utilizaremos a versão 3.0 do Django, vamos já utilizar a versão 3.8.2 do Python lançada em 24 de fevereiro de 2020.

### **Virtualenv**

Agora que temos a versão 3.8 do Python instalada, vamos instalar o Virtualenv para nos ajudar a organizar e isolar os ambientes de desenvolvimento dentro da nossa máquina. Com isso, a gente ganha a possibilidade de trabalhar em diversos projetos utilizando diferentes versões do Python e do Django na mesma máquina, sem correr o risco de que ocorram conflitos ou outros problemas relacionados a versão dos pacotes. Para instalar o Virtualenv vamos utilizar o seguinte comando:

```bash
$ sudo apt install virtualenv
```

Com o Virtualenv instalado, podemos criar um ambiente totalmente isolado para instalar as dependências relacionadas a cada projeto. Com o terminal aberto na pasta `home`, vamos criar um diretório chamado `dev` para mantermos nossos projetos. Para criar e acessar o diretório, vamos utilizar os seguintes comandos:

```bash
$ cd
$ mkdir dev
$ cd dev
```

Vamos utilizar o comando `cd` para garantir que estamos na pasta `home` do usuário logado em sua máquina e depois utilizamos o comando `mkdir dev` para criar a pasta `dev`. Com o diretório `dev` criado, vamos entrar nele utilizando o comando `cd dev` e criar o diretório que dará nome ao nosso projeto. Para isso, vamos utilizar os seguintes comandos:

```bash
$ mkdir controle-visitantes
$ cd controle-visitantes
```

Feito isso, vamos criar o nosso ambiente virtual utilizando o Virtualenv. Ele cria para nós um diretório onde ficam os pacotes instalados e utilizados dentro do nosso projeto. Vamos utilizar o comando:

```bash
$ virtualenv -p python3.8 env
```

{% hint style="info" %}
O comando `virtualenv` nos deixa passar alguns argumentos para deixar o ambiente mais próximo das nossas necessidades. Neste caso, estamos dizendo para ele qual a versão do Python queremos utilizar \(`Python 3.8`\) e o nome do nosso ambiente, que será `env`
{% endhint %}

Após criarmos o ambiente, temos que ativá-lo. Esse passo deve ser executado sempre que você for começar a trabalhar no projeto, pois nossa máquina contém pacotes e versões diferentes das que estão em nosso ambiente virtual e, por isso, precisamos avisar para nosso terminal de onde ele deverá executar os pacotes e até mesmo o Python. Para ativar nosso ambiente, utilizaremos o comando:

```bash
$ source env/bin/activate
```

Após execução do comando e ativação do ambiente, você deverá visualizar em seu terminal algo como:

```bash
(env)$
```

### **Pip**

O pip é um gerenciador de pacotes bastante utilizado no mundo Python e ele vem instalado sempre que criamos um novo ambiente virtual utilizando o virtualenv. Sendo assim, não vamos precisar instalá-lo, mas precisamos lembrar que só teremos a possibilidade de utilizar o comando `pip` com o ambiente virtual ativado.

### **Django framework**

Com o ambiente virtual ativado, vamos começar a instalar as dependências do projeto. Por enquanto temos apenas o Django Framework, que é quem vai nos ajudar e muito nessa jornada. Para instalá-lo, vamos utilizar o Pip, nosso gerenciador de pacotes do universo Python utilizando o comando `pip`. Bem tranquilo, não?

Para que a gente não tenha problemas referentes à versão utilizada, vamos especificar ao Pip qual versão do Django queremos instalar:

```bash
(env)$ pip install Django==3.0.0
```

Ao fim de tudo e tendo sucesso em todas as instalações, temos o ambiente pronto para começar a desenvolver o projeto. 

Sendo assim, bora pro próximo capítulo!


---
description: >-
  O primeiro capítulo aborda quem é o instrutor, o que será desenvolvido no
  decorrer do projeto, o que é a linguagem Python e como instalar todas os
  recursos necessários ao início do projeto.
---

# Capítulo 01

## Conhecendo o instrutor e sabendo mais sobre o curso

### **Sobre o instrutor**

Olá, meu nome é Thiago Brasil e na internet eu sou o @tchaguitos.

Cursei Ciência da Computação e sou desenvolvedor full-stack há aproximadamente 3 anos. Nesse tempo trabalhei principalmente com as linguagens Python e Javascript e me aventurei em vários projetos utilizando Django framework, NodeJS, React, Angular, Vue, React Native e outras tecnologias dos mundos back-end, front-end e mobile híbrido.

### **Sobre o curso**

No curso "Django Framework na prática" você vai iniciar sua carreira como desenvolvedor full-stack utilizando a linguagem Python e desenvolver aplicações web seguras e escaláveis numa velocidade incrível.

Nesse curso a gente vai construir junto uma aplicação web do zero com Python utilizando o framework Django. Essa nossa aplicação consiste num sistema de controle de visitantes para condomínios. Nossa aplicação deverá ser capaz de cadastrar porteiros que terão acesso a uma dashboard para cadastro de visitantes do condomínio. 

Depois desse curso você estará apto para desenvolver aplicações web robustas utilizando Django framework, tendo visão ampla das principais funcionalidades e facilidades que o framework nos permite aproveitar.

## A linguagem de programação Python

Python é uma linguagem de programação de código aberto poderosa, rápida, amigável e bastante popular. Com ela é possível desenvolver sistemas web, trabalhar com análise de dados, inteligência artificial e machine learning, além de desenvolver aplicativos móveis e aplicações para desktop.

A linguagem foi criada por Guido van Rossum em 1991 com o objetivo de ser uma linguagem produtiva e legível, de modo que fosse fácil para humanos trabalharem em seus códigos, tornando menos custoso o processo de desenvolvimento e manutenção de sistemas.

Além da grande variedade de ferramentas que a própria linguagem disponibiliza, Python tem se tornado cada vez mais comum devido a sua utilização em áreas como Data Science, Machine Learning e Inteligência Artificial e, com isso, ganhado diversas bibliotecas e módulos que facilitam o dia a dia do desenvolvedor.

E ah, por último e não menos importante: Python tem caminhado rumo ao status de linguagem de programação mais popular do mundo!

## O que é Django Framework?

Django é um framework para construção de aplicações web escrito em Python gratuito e de código aberto. Um framework tem o objetivo de agrupar funcionalidades que são utilizadas com certa frequência na construção de aplicações exatamente com o intuito de economizar tempo dos desenvolvedores.

O Django, por sua vez, foi construído por desenvolvedores experientes e projetado pra resolver grande parte dos problemas existentes no desenvolvimento web. Sendo assim, a gente pode se concentrar em escrever a nossa aplicação sem a necessidade de ficar reinventando a roda.

Dentre as empresas que utilizam o Django no mundo, temos exemplos grandes como Instagram, Pinterest e a Mozilla. Aqui no Brasil existem diversas empresas que utilizam como a Olist, a Magalu e o pessoal da Globo.com.

## Instalando e configurando o ambiente para iniciar o projeto

Antes de tudo, a gente precisa preparar o nosso ambiente. Esse nosso ambiente é bem simples e tem como dependência apenas quatro itens. São eles:

* Python 3.6
* Pip
* Virtualenv
* Django framework

### **Python 3.6**

Para instalar o Python na versão 3.6, a gente precisa abrir o nosso terminal e utilizar os seguintes comandos:

```bash
$ sudo apt-get update
$ sudo apt-get install python3.6
```

### **Virtualenv**

Antes de instalar o Django, vamos instalar o Virtualenv para nos ajudar a organizar e isolar os ambientes de desenvolvimento dentro da nossa máquina. Com isso, a gente ganha a possibilidade de trabalhar em diversos projetos utilizando diferentes versões do Python e do Django na mesma máquina, sem correr o risco de ocorrerem conflitos ou outros problemas. Para instalar o Virtualenv é muito fácil:

```bash
$ sudo apt-get install virtualenv
```

Com o Virtualenv instalado, podemos criar um ambiente totalmente isolado para instalar as dependências relacionadas ao nosso projeto. Com o terminal aberto na pasta Home, vamos criar um diretório chamado "dev" para mantermos nossos projetos. Para criar e acessar o diretório, vamos utilizar o seguinte comando:

```bash
$ cd home
$ mkdir dev
$ cd dev
```

Com o diretório "dev" criado, vamos criar o diretório que dará nome ao nosso projeto. Para isso, basta utilizar o seguinte comando:

```bash
$ mkdir controle-visitantes
$ cd controle-visitantes
```

Feito isso, vamos criar o nosso ambiente virtual através do Virtualenv. Ele cria para nós um diretório onde ficam os pacotes instalados que serão utilizados dentro do nosso projeto. Para isso, basta utilizar um comando:

```bash
$ virtualenv -p python3.6 env
```

> O comando "virtualenv" nos possibilita passar alguns argumentos para deixar o ambiente mais próximo das nossas necessidades. Neste caso, estamos dizendo para ele qual a versão do Python queremos utilizar \(Python 3.6\) e o nome do nosso ambiente, que será "env".

Após criarmos o ambiente, temos que ativá-lo. Esse passo deve ser executado sempre que você for começar a trabalhar no projeto, pois nossa máquina contém pacotes e versões diferentes das que estão em nosso ambiente virtual e, por isso, precisamos avisar para nosso terminal de onde ele deverá executar os pacotes e até mesmo o Python. Para ativar nosso ambiente, basta utilizar o comando:

```bash
$ source env/bin/activate
```

Após execução do comando e ativação do ambiente, você deverá visualizar em seu terminal algo como:

```bash
(env)$
```

### **Pip**

O pip é um gerenciador de pacotes utilizado no mundo Python e pode ser instalado facilmente utilizando o seguinte comando:

```bash
$ sudo apt-get install python3-pip
```

### **Django framework**

Com o ambiente virtual ativado, vamos começar a instalar as dependências do projeto. Por enquanto temos apenas o Django Framework, que é quem vai nos ajudar e muito nessa jornada! Para instalá-lo, vamos utilizar o Pip, nosso gerenciador de pacotes do universo Python utilizando o comando "pip". Bem fácil, não?

Para que a gente não tenha problemas referentes à versão utilizada, vamos especificar ao Pip qual versão do Django queremos instalar:

```bash
(env)$ pip install Django==3.0.0
```

Ao fim de tudo e tendo sucesso em todas as instalações, temos o ambiente pronto para começar a desenvolver o projeto.

Então bora pra próxima aula!


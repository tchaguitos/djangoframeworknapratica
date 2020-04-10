# Capítulo 14 - dev

## Bloqueando o acesso para usuários não autenticados nas nossas views

Estamos chegando na reta final do nosso projeto. Todos os requisitos descritos estão implementados e agora nós vamos cuidar de alguns requisitos que são chamados não funcionais. Esse tipo de requisito raramente é descrito mas está presente em praticamente todas as aplicações web existentes: telas de login, telas de logout e o bloqueio de acesso às páginas para usuários que não estejam autenticados.

Daqui pra frente cuidaremos os requisitos falamos e vamos conhecer alguns recursos bem interessantes do Django que vão nos ajudar a poupar bastante tempo. 

Pense na dashboard que estamos criando: as informações que estamos exibindo ali são confidenciais e não devem ficar expostas para que qualquer um possa escontrá-las na web. Desta forma, nós precisamos bloquear as informações para que somente usuários autenticados com e-mail e senha possam ter acesso e visualizar essas informações.

### Conhecendo o decorator login\_required

Como quase tudo que precisamos para desenvolver uma aplicação web, o Django também nos fornece um caminho rápido para que a gente consiga implementar essa função. Nós vamos utilizar o decorator login\_required para tornar nossas views acessíveis somente após autenticação. Caso o usuário não esteja autenticado, não poderá acessar as views e será direcionado para uma tela de login.

Um decorator nada mais é que um método que envolve e modifica comportamentos de uma função. E é isso que estamos fazendo: pedindo que o decorator login\_required faça a função ser acessível com autenticação. continuar...

## Utilizando o sistema de autenticação do Django para nos fornecer a view de login

### Criando a URL

* Utilizando o método as\_view\(\)

## Criando o template de login

* Renderizando formulário de login

## Alterando a URL padrão para login e redirecionamento após login

* Alterando o settings.py

## Adicionando mensagem de erro em formulário de login

## Criando URL para logout

## Criando template de logout

## Inserindo link para logout em dashboard


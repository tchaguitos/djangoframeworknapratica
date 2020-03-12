# Capítulo 06

## Criando tela para registro de novo visitante

Assim como quando registramos um novo visitante através do Admin, precisaremos de um formulário para inserção das informações. Por isso, vamos trabalhar agora na tela que terá a responsabilidade de exibir um formulário e registrar o visitante em nosso banco de dados.

Uma view é um tipo de função dentro da aplicação que geralmente serve um template específico. Até agora escrevemos somente a view que nos disponibiliza o template inicial da dashboard - `index.html`. A view que escrevemos apenas busca informações e renderiza o template utilizando as informações buscadas.

A próxima view que vamos escrever, chamada de `registrar_visitante`, terá a responsabilidade de exibir um formulário, receber e tratar uma requisição do tipo POST, validar o formulário com base das informações enviadas na requisição e salvar o novo visitante no banco de dados. Não se assuste, você vai ver como o Django nos ajuda abstraindo a maior parte desses requisitos.

## Criando view para registrar visitante

Como você já deve ter percebido, existe um roteiro a ser seguido quando vamos criar novas views para nosso sistema web com Django, sendo o primeiro passo a criação da função de view no arquivo `views.py`. Como nossa função diz respeito ao registro de um novo vistante, vamos trabalhar dentro do aplicativo visitantes.

Vamos abrir o arquivo `views.py` do aplicativo visitantes e escrever a função de view `registrar_visitante`, que deverá renderizar o template `registrar_visitante.html`. Por hora, nossa view fará apenas isso e ficará assim:

```python
from django.shortcuts import render

def registrar_visitante(request):

    contexto = {}
    
    return render(request, "registrar_visitante.html", contexto)
```

Assim como fizemos anteriormente, vamos baixar o template e agora colocá-lo na pasta **templates** localizada na raíz do nosso projeto:

{% file src="../.gitbook/assets/registrar\_visitante.html.zip" caption="Iniciar o download" %}

### Criando URL para mapear view

Quando criamos nossa primeira view, criamos também uma URL com função de mapear a view para acessarmos através do navegador. Caso não se lembre do processo, não se preocupe, pois vamos repetí-lo agora.

Vamos abrir o arquivo `urls.py` do nosso projeto e, abaixo da URL de nome **index**, utilizando a função `path`, vamos criar a URL de nome **registrar\_visitante** que deverá mapear a função de view ****`registrar_visitante`. Não podemos nos esquecer de importar as views do aplicativo visitantes!

O arquivo `urls.py` ficará assim:

```python
from django.urls import path
from django.contrib import admin

import usuarios.views
import visitantes.views

urlpatterns = [
    # codigo acima omitido...
    
    path(
        "",
        usuarios.views.index,
        name="index",
    ),
    
    path(
        "registrar-visitante",
        visitantes.views.registrar_visitante,
        name="registrar_visitante",
    )
]
```

Abra seu navegador e acesse [http://127.0.0.1:8000/registrar-vistante/](http://127.0.0.1:8000/registrar-vistante/) para verificar se está tudo funcionando corretamente. Se sim, o template baixado será exibido no navegador.

## Adaptando nossos templates para trabalhar com a template engine do Django

Temos agora duas views que renderizam dois templates diferentes e expõem funcionalidades diferentes: uma delas, que é a página inicial da dashboard, exibe os visitantes registrados, e a segunda deverá possibilitar o registro de novos visitantes. Antes de seguir adiante, vamos realizar algumas alterações em nossos templates para que possamos aproveitar melhor as funcionalidades do framework que estamos utilizando.

Além das funcionalidades que falamos e exploramos, a engine de templates do Django também nos dá a possibilidade de reaproveitarmos trechos de código contidos em outros templates. No nosso caso, se você observar os templates `index.html` e `registrar_visitante.html`, vai notar que existem partes iguais nos dois templates e, para evitar isso, a engine de templates do Django nos fornece as tags `{% extends %}` e `{% block %}`.

### Criando o template base

Antes de tudo, vamos criar um arquivo com nome de `base.html` na pasta **templates** e copiar o conteúdo do arquivo `index.html` para ele. O objetivo do nosso template `base.html` é armazenar a parte comum a todos os templates, sendo assim, temos que manter o menu lateral, a barra superior e o rodapé. O que podemos chamar de parte central do nosso template, que é a parte que em um template exibe uma tabela e no outro um formulário, será trocada de acordo com a view acessada. Após copiar o conteúdo do arquivo `index.html` para o arquivo `base.html`, vamos deixá-lo de lado um pouquinho.

### Adaptando template index

Com o template `base.html` criado, vamos fazer algumas adaptações em nosso template `index.html` para garantir que ele seja exibido corretamente fazendo uso da engine de templates do Django. Apague todo o conteúdo existente no arquivo `index.html` deixando apenas o conteúdo dentro do elemento HTML `<div class="container-fluid">`. O arquivo `index.html` ficará assim:

```python
<div class="container-fluid">
    <div class="d-sm-flex align-items-center justify-content-between mb-4">
        <h1 class="h3 mb-0 text-gray-800">{{ nome_pagina }}</h1>
    </div>
                  
    <div class="row">
        <div class="col-xl-3 col-md-6 mb-4">
            <div class="card border-left-success shadow h-100 py-2">
                <div class="card-body">
                    <div class="row no-gutters align-items-center">
                        <div class="col mr-2">
                            <div class="text-xs font-weight-bold text-success text-uppercase mb-1">Visitantes no condomínio</div>
                            <div class="h5 mb-0 font-weight-bold text-gray-800">8</div>
                        </div>
                                        
                        <div class="col-auto">
                            <i class="fas fa-user-check fa-2x text-gray-300"></i>
                        </div>
                    </div>
                </div>
            </div>
        </div>
                        
        <div class="col-xl-3 col-md-6 mb-4">
            <div class="card border-left-warning shadow h-100 py-2">
                <div class="card-body">
                    <div class="row no-gutters align-items-center">
                        <div class="col mr-2">
                            <div class="text-xs font-weight-bold text-warning text-uppercase mb-1">Visitantes aguardando autorização</div>
                            <div class="h5 mb-0 font-weight-bold text-gray-800">15</div>
                        </div>
                                        
                        <div class="col-auto">
                            <i class="fas fa-user-lock fa-2x text-gray-300"></i>
                        </div>
                    </div>
                </div>
            </div>
        </div>
      
        <div class="col-xl-3 col-md-6 mb-4">
            <div class="card border-left-primary shadow h-100 py-2">
                <div class="card-body">
                    <div class="row no-gutters align-items-center">
                        <div class="col mr-2">
                            <div class="text-xs font-weight-bold text-primary text-uppercase mb-1">Número de visitantes na semana</div>
                            <div class="h5 mb-0 font-weight-bold text-gray-800">18</div>
                        </div>
                        
                        <div class="col-auto">
                            <i class="fas fa-user-friends fa-2x text-gray-300"></i>
                        </div>
                    </div>
                </div>
            </div>
        </div>

        <div class="col-xl-3 col-md-6 mb-4">
            <div class="card border-left-info shadow h-100 py-2">
                <div class="card-body">
                    <div class="row no-gutters align-items-center">
                        <div class="col mr-2">
                            <div class="text-xs font-weight-bold text-info text-uppercase mb-1">Número de visitantes no mês</div>
                            <div class="h5 mb-0 font-weight-bold text-gray-800">37</div>
                        </div>
                        
                        <div class="col-auto">
                            <i class="fas fa-users fa-2x text-gray-300"></i>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </div>
                    
    <div class="card shadow mb-4">
        <div class="card-header py-3">
            <h6 class="m-0 font-weight-bold text-primary">Lista de visitantes</h6>
        </div>

        <div class="card-body">
            <div class="table-responsive">
                <table class="table table-bordered dataTable" id="dataTable" width="100%" cellspacing="0" role="grid" aria-describedby="dataTable_info" style="width: 100%;">
                    <thead>
                        <th>Nome</th>
                        <th>CPF</th>
                        <th>Horário de chegada</th>
                        <th>Horário da autorização</th>
                        <th>Autorizado por</th>
                        <th>Mais detalhes</th>
                    </thead>

                    <tbody>
                        {% for visitante in todos_visitantes %}
                            <td>{{ visitante.nome_completo }}</td>
                            <td>{{ visitante.cpf }}</td>
                            <td>{{ visitante.horario_chegada }}</td>
                            <td>{{ visitante.horario_autorizacao }}</td>
                            <td>{{ visitante.morador_resposavel }}</td>
                            <td>
                                <a href="#">
                                    Ver detalhes
                                </a>
                            </td>
                        {% endfor %}
                    </tbody>
                </table>
            </div>
        </div>
    </div>
</div>
```

Com os templates devidamente separados, vamos trabalhar agora nas adaptações necessárias ao template `index.html`. O primeiro passo é inserirmos a tag `{% extends %}` no início do nosso arquivo, que é quem dirá ao Django que é necessário estender o template. A tag `{% extends %}` funciona de modo que precisamos identificar o template "pai" do template estendido. Isto é, neste caso, o template `index.html` será estentido pelo template `base.html`, sendo este o "seu pai". Na primeira linha do arquivo `index.html` insira o trecho `{% extends "base.html" %}`.

Além disto, precisamos também dizer ao Django qual trecho deverá ser utilizado para substituição. Faremos isso utilizando as tags `{% block %}` e `{% endblock %}` passando um nome a elas. Logo abaixo da tag `{% extends base.html %}` vamos inserir a tag `{% block conteudo %}` e ao final do arquivo a tag `{% endblock conteudo %}`. Fazendo isso estamos deixando claro para o Django qual trecho deverá ser colocado no template `base.html` quando acessarmos a view que renderiza o template `index.html`.  Abaixo o template `index.html` após as adaptações:

```python
{% extends "base.html" %}

{% block conteudo %}
<div class="container-fluid">
    <div class="d-sm-flex align-items-center justify-content-between mb-4">
        <h1 class="h3 mb-0 text-gray-800">{{ nome_pagina }}</h1>
    </div>
                  
    <div class="row">
        <div class="col-xl-3 col-md-6 mb-4">
            <div class="card border-left-success shadow h-100 py-2">
                <div class="card-body">
                    <div class="row no-gutters align-items-center">
                        <div class="col mr-2">
                            <div class="text-xs font-weight-bold text-success text-uppercase mb-1">Visitantes no condomínio</div>
                            <div class="h5 mb-0 font-weight-bold text-gray-800">8</div>
                        </div>
                                        
                        <div class="col-auto">
                            <i class="fas fa-user-check fa-2x text-gray-300"></i>
                        </div>
                    </div>
                </div>
            </div>
        </div>
                        
        <div class="col-xl-3 col-md-6 mb-4">
            <div class="card border-left-warning shadow h-100 py-2">
                <div class="card-body">
                    <div class="row no-gutters align-items-center">
                        <div class="col mr-2">
                            <div class="text-xs font-weight-bold text-warning text-uppercase mb-1">Visitantes aguardando autorização</div>
                            <div class="h5 mb-0 font-weight-bold text-gray-800">15</div>
                        </div>
                                        
                        <div class="col-auto">
                            <i class="fas fa-user-lock fa-2x text-gray-300"></i>
                        </div>
                    </div>
                </div>
            </div>
        </div>
      
        <div class="col-xl-3 col-md-6 mb-4">
            <div class="card border-left-primary shadow h-100 py-2">
                <div class="card-body">
                    <div class="row no-gutters align-items-center">
                        <div class="col mr-2">
                            <div class="text-xs font-weight-bold text-primary text-uppercase mb-1">Número de visitantes na semana</div>
                            <div class="h5 mb-0 font-weight-bold text-gray-800">18</div>
                        </div>
                        
                        <div class="col-auto">
                            <i class="fas fa-user-friends fa-2x text-gray-300"></i>
                        </div>
                    </div>
                </div>
            </div>
        </div>

        <div class="col-xl-3 col-md-6 mb-4">
            <div class="card border-left-info shadow h-100 py-2">
                <div class="card-body">
                    <div class="row no-gutters align-items-center">
                        <div class="col mr-2">
                            <div class="text-xs font-weight-bold text-info text-uppercase mb-1">Número de visitantes no mês</div>
                            <div class="h5 mb-0 font-weight-bold text-gray-800">37</div>
                        </div>
                        
                        <div class="col-auto">
                            <i class="fas fa-users fa-2x text-gray-300"></i>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </div>
                    
    <div class="card shadow mb-4">
        <div class="card-header py-3">
            <h6 class="m-0 font-weight-bold text-primary">Lista de visitantes</h6>
        </div>

        <div class="card-body">
            <div class="table-responsive">
                <table class="table table-bordered dataTable" id="dataTable" width="100%" cellspacing="0" role="grid" aria-describedby="dataTable_info" style="width: 100%;">
                    <thead>
                        <th>Nome</th>
                        <th>CPF</th>
                        <th>Horário de chegada</th>
                        <th>Horário da autorização</th>
                        <th>Autorizado por</th>
                        <th>Mais detalhes</th>
                    </thead>

                    <tbody>
                        {% for visitante in todos_visitantes %}
                            <td>{{ visitante.nome_completo }}</td>
                            <td>{{ visitante.cpf }}</td>
                            <td>{{ visitante.horario_chegada }}</td>
                            <td>{{ visitante.horario_autorizacao }}</td>
                            <td>{{ visitante.morador_resposavel }}</td>
                            <td>
                                <a href="#">
                                    Ver detalhes
                                </a>
                            </td>
                        {% endfor %}
                    </tbody>
                </table>
            </div>
        </div>
    </div>
</div>
{% endblock conteudo %}
```

### Adaptando template base

Quando criamos o template `base.html`, copiamos o conteúdo de `index.html` para ele e o deixamos de lado, mas agora é hora de trabalharmos nele novamente. O que temos que fazer é substituir o elemento HTML `<div class="container-fluid">` pelas tags `{% block conteudo %}` e `{% endblock conteudo %}.` O template `base.html` ficará assim:

```python
<!DOCTYPE html>

{% load static %}

<html lang="pt-BR">
    <head>
        <meta charset="utf-8">
        <meta http-equiv="X-UA-Compatible" content="IE=edge">
        <meta name="viewport" content="width=device-width, initial-scale=1, shrink-to-fit=no">
        <meta name="description" content="">
        <meta name="author" content="">
        
        <title>Controle de Visitantes</title>
        
        <link href="https://fonts.googleapis.com/css?family=Nunito:200,200i,300,300i,400,400i,600,600i,700,700i,800,800i,900,900i" rel="stylesheet">
        
        <link href="{% static 'css/sb-admin-2.min.css' %}" rel="stylesheet">    
        <link href="{% static 'vendor/fontawesome-free/css/all.min.css' %}" rel="stylesheet" type="text/css">
    </head>

    <body id="page-top">
        <div id="wrapper">
            <ul class="navbar-nav bg-gradient-primary sidebar sidebar-dark accordion" id="accordionSidebar">
                <a class="sidebar-brand d-flex align-items-center justify-content-center" href="#">
                    <div class="sidebar-brand-icon rotate-n-15">
                        <i class="fas fa-user-shield"></i>
                    </div>
                    
                    <div class="sidebar-brand-text">Controle de Visitantes</div>
                </a>
                
                <hr class="sidebar-divider my-0">
                
                <li class="nav-item">
                    <a class="nav-link" href="#">
                        <i class="fas fa-fw fa-tachometer-alt"></i>
                        <span>Início</span>
                    </a>
                </li>
                
                <hr class="sidebar-divider">
                
                <div class="sidebar-heading">
                    Menu
                </div>
                
                <li class="nav-item">
                    <a class="nav-link" href="#">
                        <i class="fas fa-user-clock"></i>
                        <span>Visitantes</span>
                    </a>
                </li>
            </ul>

            {% block conteudo %} {% endblock conteudo %}

            <div class="modal fade" id="logoutModal" tabindex="-1" role="dialog" aria-labelledby="exampleModalLabel" aria-hidden="true">
                <div class="modal-dialog" role="document">
                    <div class="modal-content">
                        <div class="modal-header">
                            <h5 class="modal-title" id="exampleModalLabel">Você realmente deseja sair?</h5>
                            
                            <button class="close" type="button" data-dismiss="modal" aria-label="Close">
                                <span aria-hidden="true">×</span>
                            </button>
                        </div>
                        
                        <div class="modal-body">Selecione "sair" se realmente deseja sair</div>
                        
                        <div class="modal-footer">
                            <button class="btn btn-secondary" type="button" data-dismiss="modal">Cancelar</button>
                            <a class="btn btn-primary" href="#">Sair</a>
                        </div>
                    </div>
                </div>
            </div>
        </div>
            
        <script src="{% static 'vendor/jquery/jquery.min.js' %}"></script>
        <script src="{% static 'vendor/bootstrap/js/bootstrap.bundle.min.js' %}"></script>
        <script src="{% static 'js/sb-admin-2.min.js' %}"></script>
    </body>
</html>
```

### Adaptando template registrar\_visitante

Faremos o mesmo que fizemos com o template `index.html`, deixando apenas o conteúdo existente dentro do elemento HTML `<div class="container-fluid">` e inserindo as tags `{% extends "base.html" %}`, `{% block conteudo %}` e `{% endblock conteudo %}`. O template `registrar_visitante.html` ficará assim:

```python
{% extends "base.html" %}

{% block conteudo %}
<div class="container-fluid">
    <div class="d-sm-flex align-items-center justify-content-between mb-4">
        <h1 class="h3 mb-0 text-gray-800">{{ nome_pagina }}</h1>
    </div>
    
    <div class="card shadow mb-4">
        <div class="card-header py-3">
            <h6 class="m-0 font-weight-bold text-primary">Registrar visitante</h6>
        </div>
        
        <div class="card-body">
            <div class="container">
                <form class="user">
                    <div class="form-group">
                        <input type="email" class="form-control form-control-user form-sm" placeholder="E-mail">
                    </div>

                    <div class="form-group">
                        <input type="password" class="form-control form-control-user"placeholder="Password">
                    </div>

                    <a href="index.html" class="btn btn-primary btn-user">
                        Login
                    </a>
                </form>
            </div>
        </div>
    </div>
</div>
{% endblock conteudo %}
```

Caso prefira, você pode fazer download da pasta templates com as alterações realizadas até aqui clicando no link abaixo:

{% file src="../.gitbook/assets/templates.zip" caption="Iniciar o download" %}

Agora que adaptamos todos os nossos templates, vamos voltar ao desenvolvimento da view responsável por registrar nossos visitantes!

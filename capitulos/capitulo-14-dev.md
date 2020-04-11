# Capítulo 14 - dev

## Bloqueando o acesso para usuários não autenticados nas nossas views

Estamos chegando na reta final do nosso projeto. Todos os requisitos descritos estão implementados e agora nós vamos cuidar de alguns requisitos que são chamados não funcionais. Esse tipo de requisito raramente é descrito mas está presente em praticamente todas as aplicações web existentes: telas de login, telas de logout e o bloqueio de acesso às páginas para usuários que não estejam autenticados.

Daqui pra frente cuidaremos os requisitos falamos e vamos conhecer alguns recursos bem interessantes do Django que vão nos ajudar a poupar bastante tempo. O Django possui embutido em suas funcionalidades, alguns módulos voltados para a criação, administração e autenticação de usuários, conhecido por sistema de autenticação do Django. O comando `createsuperuser`, por exemplo, faz parte de suas funcionalidades.

Pense na dashboard que estamos criando: as informações que estamos exibindo ali são confidenciais e não devem ficar expostas para que qualquer um possa escontrá-las na web. Desta forma, nós precisamos bloquear as informações para que somente usuários autenticados com e-mail e senha possam ter acesso e visualizar essas informações.

### Conhecendo o decorator login\_required

Como quase tudo que precisamos para desenvolver uma aplicação web, o Django também nos fornece um caminho rápido para que a gente consiga implementar essa função. Nós vamos utilizar o decorator login\_required para tornar nossas views acessíveis somente após autenticação. Caso o usuário não esteja autenticado, não poderá acessar as views e será direcionado para uma tela de login.

Um decorator nada mais é que um método que envolve e modifica comportamentos de uma função. É isso que estamos fazendo: pedindo que o decorator `login_required` faça a função ser acessível somente após autenticação e, com isso, estamos alterando o comportamento da função.

Para utilizar esse decorator vamos primeiro importá-lo no topo do arquivo `views.py`:

```python
from django.shortcuts import render
from django.contrib.auth.decorators import login_required

# código abaixo omitido
```

Agora tudo que precisamos fazer é colocar um `@` antes do nome do decorator em cima da função `index`. O código ficará assim:

```python
@login_required
def index(request):
    
    visitantes = Visitante.objects.order_by(
        "-horario_chegada"
    )
    
    # filtrando os visitantes por status
    visitantes_aguardando = visitantes.filter(
        status="AGUARDANDO"
    )

    visitantes_em_visita = visitantes.filter(
        status="EM_VISITA"
    )

    visitantes_finalizado = visitantes.filter(
        status="FINALIZADO"
    )
    
    hora_atual = datetime.now()
    mes_atual = hora_atual.month
    
    visitantes_mes = visitantes.filter(
        horario_chegada__month=mes_atual
    )
    
    context = {
        "nome_pagina": "Página inicial",
        "visitantes": visitantes,
        "visitantes_aguardando": visitantes_aguardando.count(),
        "visitantes_em_visita": visitantes_em_visita.count(),
        "visitantes_finalizado": visitantes_finalizado.count(),
        "visitantes_mes": visitantes_mes.count(),
    }
    
    return render(request, "index.html", context)
```

Agora, se você tentar acessar a URL [http://127.0.0.1:8000/](http://127.0.0.1:8000/) sem estar autenticado, o Django irá exibir uma mensagem de erro. Esse erro ocorre porque quando utilizamos o decorator `login_required`, precisamos configurar algumas URLs que vão servir para o Django redirecionar o usuário para o login e uma página após o login. 

{% hint style="success" %}
Caso esteja autenticado, vá até o admin e clique em "encerrar sessão" ou cliquei nesse link: [http://127.0.0.1:8000/admin/logout/](http://127.0.0.1:8000/admin/logout/)
{% endhint %}

## Alterando a URL padrão para login e redirecionamento após login

Nosso primeiro passo será definir as informações citadas anteriormente no arquivo `settings.py`. Vamos configurar as variáveis `LOGIN_URL` e `LOGIN_REDIRECT_URL`. e definir seus valores como `"login"` e `"index"`. O arquivo ficará assim:

```python
# código acima omitido

LOGIN_URL = "login"
LOGIN_REDIRECT_URL = "index"

STATICFILES_DIRS = [
    os.path.join(BASE_DIR, "static")
]

# código abaixo omitido
```

Não se preocupe com a URL login. Vamos criá-la no próximo passo.

## Utilizando o sistema de autenticação do Django para nos fornecer a view de login

Conforme dito, vamos utilizar o sistema de autenticação do Django para nos ajudar com todo código necessário para autenticar e deslogar os usuários da nossa dashboard. Para começar vamos abrir o arquivo `urls.py` e importar os seguinte módulo:

```python
from django.contrib import admin
from django.urls import path

from django.contrib.auth import views as auth_views

import dashboard.views
import visitantes.views

# código abaixo omitido
```

O que estamos fazendo é importar o arquivo de views do módulo `django.contrib.auth`. Fazendo isso, nós podemos utilizar as funções e classes disponíveis no módulo `django.contrib.auth.views`. Abaixo da função `path()` que define a URL para o admin, vamos definir a nossa URL de login:

```python
urlpatterns = [
    path("admin/", admin.site.urls),

    path(
        "login/",
        auth_views.LoginView.as_view(
            template_name="login.html"
        ),
        name="login"
    ),
    
    # código abaixo omitido
]
```

Como importamos o arquivo inteiro com o nome de `auth_views`, vamos acessar suas funções e classes por meio desse nome. Note que no lugar da view que deveríamos passar para a função `path()`, estamos passando uma classe existente no módulo `auth_views` e utilizando seu método `as_view()`. Esse método nos permite utilizar as views padrões do Django para autenticação de modo que a gente consiga criar um template personalizado. O argumento `template_name` serve para que a gente diga para o Django qual template deve ser utilizado na view.

Com essa configuração, já temos uma URL de login. Agora precisamos de um template, claro.

## Criando o template de login

Vamos criar o arquivo login.html com o seguinte código:

```markup
<!DOCTYPE html>

{% load static %}
{% load widget_tweaks %}

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

<body class="bg-gradient-primary">
    <div class="container">
        <div class="row justify-content-center">
            <div class="col-xl-10 col-lg-12 col-md-9">
                <div class="card o-hidden border-0 shadow-lg my-5">
                    <div class="card-body p-0">
                        <div class="row">
                            <div class="col-lg-6 d-none d-lg-block bg-login-image"></div>

                            <div class="col-lg-6">
                                <div class="p-5">
                                    <div class="text-left mb-5">
                                        <h1 class="h4 text-gray-900 mb-1">Seja bem-vindo!</h1>

                                        <p>Faça login para continuar</p>
                                    </div>

                                    <form method="post" class="user">
                                        <!-- ué cadê o formulário?  -->

                                        <button class="btn btn-primary btn-user btn-block" type="submit">
                                            <span class="text">Acessar sistema</span>
                                        </button>
                                    </form>
                                </div>
                            </div>
                        </div>
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

### Renderizando formulário de login

Quando utilizamos a classe `LoginView` o Django nos dá tudo que precisamos parar exibir o template, tratar a requisição, os possíveis erros do formulário e ainda autenticar o usuário. Junto disso tudo, temos a possibilidade de utilização da variável form que representa nosso formulário de autenticação.

Quando criamos nosso modelo personalizado de usuário, o Django preparou um formulário baseado nesse modelo, de forma bem parecida com que fizemos nos formulários que criamos. Vamos utilizar a mesma estratégia que utilizamos para renderizar o formulário de registro de visitante. O código do formulário ficará assim:

```markup
<div class="form-row">
    {% csrf_token %}
                        
    {% for field in form %}
        <div class="form-group col-md-12">
            {% render_field field placeholder=field.label class="form-control form-control-user" %}
        </div>
    {% endfor %}
</div>
```

Vamos acessar novamente a URL [http://127.0.0.1:8000/dashboard](http://127.0.0.1:8000/dashboard) e agora podemos notar que fomos direcionados para a tela de login que acabamos de criar.

## Adicionando mensagem de erro em formulário de login

Agora que estamos exibindo

```markup
{% if form.errors %}
    <div class="alert alert-dismissible alert-warning" role="alert">
        E-mail ou senha incorretos
        
        <button type="button" class="close" data-dismiss="alert" aria-label="Close">
            <span aria-hidden="true">&times;</span>
        </button>
    </div>
{% endif %}
```

## Criando URL para logout

## Criando template de logout

## Inserindo link para logout em dashboard


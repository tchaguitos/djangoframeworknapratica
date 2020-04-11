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

Como importamos o arquivo inteiro com o nome de `auth_views`, vamos acessar suas funções e classes por meio desse nome. Note que no lugar da view que deveríamos passar para a função `path()`, estamos passando uma classe existente no módulo `auth_views` e utilizando seu método `as_view()`. Esse método nos permite utilizar as views padrões do Django para autenticação de modo que a gente consiga criar um template personalizado.

Com essa configuração, já temos uma URL de login. Agora precisamos de um template, claro.

## Criando o template de login

* Renderizando formulário de login

## Adicionando mensagem de erro em formulário de login

## Criando URL para logout

## Criando template de logout

## Inserindo link para logout em dashboard


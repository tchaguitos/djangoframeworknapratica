# Capítulo 10

## Criando função para finalizar visita

Agora que criamos a função para autorizar a entrada do visitante, precisamos também criar a função que finaliza a visita. Para a primeira, utilizamos o formulário `AutorizaVisitanteForm` para atualiza o nome do morador responsável e definimos manualmente o status e o horário de autorização.

Desta vez, precisamos atualizar apenas o valor do atributo `horario_saida` e alterar o status para `FINALIZADO`, que é o status para quando o visitante deixa o condomínio. Sendo assim, vamos criar uma outra view que será responsável por receber um `id`, buscar um visitante com este `id` e atualizar essas informações. A view será um pouco parecida com a view `informacoes_visitante`. 

Abaixo da função `informacoes_visitante` crie e função `finalizar_visita`:

```python
# código acima omitido

def finalizar_visita(request, id):

    if request.method == "POST":
        visitante = get_object_or_404(Visitante, id=id)

        visitante.status = "FINALIZADO"
        visitante.horario_saida = datetime.now()

        visitante.save()

        messages.success(
            request,
            "Visita finalizada com sucesso"
        )

        return redirect("index")
```

A função `finalizar_visita` deverá receber um `id` como argumento e utilizar a função `get_object_or_404` para buscar o visitante do `id` que foi passado. Após isso vamos atualizar os atributos `status` e `horario_saida` diretamente e salvar o visitante. A diferença aqui é que não utilizaremos um formulário e nossa view será acessada somente através do método `POST`. Todo o resto continuará bem parecido com as funções que já escrevemos antes.

Para garantir que as operações serão realizadas somente quando o método `POST` for utilizado, vamos escrever um `if` para certificar essa informação \(`if request.method == "POST":`\) e, caso seja verdadeira, vamos executar as operações necessárias. Note que, mais uma vez, estamos utilizando o método `datetime.now()` mas, desta vez, para o atributo `horario_saida`, e setando diretamente o `status` que agora deve receber o status `FINALIZADO`. 

## Criando URL

Assim como todas as outras funções de view que escrevemos, essa também será mapeada por meio de uma URL para que a gente possa acessá-la pelo navegador. Vamos para o nosso arquivos `urls.py` e criar essa nova URL. 

A URL que irá mapear a função `finalizar_visita` será bem parecida com a URL `informacoes_visitante`, mas a diferença é que adicionaremos, após o `id`, o trecho `finalizar-visita/`. Com isso, conseguimos diferenciar qual função de view chamar para quando o usuário desejar apenas visualizar as informações de um visitante e para quando desejar finalizar uma visita. Nosso arquivo `urls.py` ficará assim:

```python
from django.contrib import admin
from django.urls import path

import usuarios.views
import visitantes.views

urlpatterns = [
    path("admin/", admin.site.urls),

    path(
        "",
        usuarios.views.index,
        name="index",
    ),

    path(
        "registrar-vistante/",
        visitantes.views.registrar_visitante,
        name="registrar_visitante",
    ),

    path(
        "visitantes/<int:id>/",
        visitantes.views.informacoes_visitante,
        name="informacoes_visitante",
    ),

    path(
        "visitantes/<int:id>/finalizar-visita/",
        visitantes.views.finalizar_visita,
        name="finalizar_visita"
    )
]
```

## Alterando template para exibir botão e modal para finalizar visita

Agora que temos a URL para onde devemos enviar uma requisição para sinalizar que queremos finalizar uma visita, vamos alterar as partes do template que vão possibilitar a interação do usuário com essa funcionalidade.

Assim como inserimos um botão para quando queremos autorizar a entrada de um visitante, vamos inserir um botão para quando quisermos finalizar a visita. Abra o template `informacoes_visitante.html` e insira o seguinte trecho de código abaixo do botão responsável por autorizar a entrada do visitante:

```markup
<a href="#" class="btn btn-warning btn-icon-split btn-sm" data-toggle="modal" data-target="#modal2">
    <span class="text">Finalizar visita</span>
                    
    <span class="icon text-white-50">
        <i class="fas fa-door-open"></i>
    </span>
</a>
```

O template ficará assim:

![](../.gitbook/assets/screenshot_2020-04-06_19-42-46.png)

Note que a estrutura é bem parecida com a que utilizamos no outro botão, mas quando observamos o atributo `data-target` podemos notar que agora ele é igual a `#modal2`. Isso porque vamos também criar um outro modal para ser exibido quando o usuário clicar no botão para finalizar uma visita. A função desse modal é obter a confirmação se é isso mesmo que o usuário deseja fazer.

Ainda no mesmo arquivo, mas agora ao final do arquivo, vamos colocar o seguinte trecho de código logo abaixo da estrutura HTML do primeiro modal:

```markup
<div class="modal fade" id="modal2" tabindex="-1" role="dialog" aria-labelledby="exampleModalLabel" aria-hidden="true">
    <div class="modal-dialog" role="document">
        <div class="modal-content">
            <div class="modal-header">
                <h5 class="modal-title" id="exampleModalLabel">Finalizar visita</h5>
                    
                <button type="button" class="close" data-dismiss="modal" aria-label="Close">
                    <span aria-hidden="true">&times;</span>
                </button>
            </div>
            
            <div class="modal-body">
                <div class="modal-body">
                    <h5 class="mb-3">
                        Realmente deseja encerrar a visita?
                    </h5>

                    <form method="post" action="{% url 'finalizar_visita' id=visitante.id %}">
                        {% csrf_token %}

                        <input hidden>

                        <div class="modal-footer">
                            <button type="button" class="btn btn-secondary" data-dismiss="modal">Cancelar</button>
                            <button type="submit" class="btn btn-primary">Finalizar visita</button>
                        </div>
                    </form>
                </div>
            </div>
        </div>
    </div>
</div>
```

Nosso segundo modal será exibido da seguinte maneira:

![](../.gitbook/assets/screenshot_2020-04-06_19-45-04.png)

Esse segundo modal deverá exibir a mensagem "Realmente deseja encerrar a visita?" e conter um formulário que enviará uma requisição do tipo `POST` para a URL que criamos anteriormente. Esse formulário precisa ter apenas o campo renderizado pela tag `{% csrf_token %}` para identificar que as requisições podem ser aceitas pelo nosso servidor.

{% hint style="info" %}
Estamos enviando uma requisição do tipo `POST` para URL pois é recomendada a utilização deste método sempre que precisamos alterar informações em nosso banco de dados
{% endhint %}

O que muda tudo aqui é o atributo `action` do formulário HTML. Graças a ele podemos direcionar um formulário para uma URL diferente da que estamos, diferentemente de como fizemos com os outros formulários. Dessa forma, conseguimos enviar uma requisição do tipo `POST` para a URL `{% url 'finalizar_visita' id=visitante.id %}` com toda informação que precisamos para identificar o visitante a ser atualizado assim que o usuário clicar no botão "Finalizar visita" dom modal confirmando a ação.

Vai em frente e teste a nova funcionalidade implementada!

## Prevenindo erros e operações desnecessárias

Nos passos anteriores, implementamos a criação da funcionalidade que finaliza as visitas dentro da nossa dashboard. Você deve ter notado que mesmo quando a visita já foi finalizada, os botões são exibidos. Isso não é bom pois o usuário pode se confundir e clicar em um dos botões, alterando as informações existentes no nosso banco de dados.

Para prevenir que isso aconteça, vamos verificar o status do visitante e exibir os botões com base no valor desse status. Funcionará assim:

* Se o visitante estiver com status `AGUARDANDO`, vamos exibir o botão para **autorizar a entrada**
* Se o visitante estiver com status `EM_VISITA`, vamos exibir o botão para **finalizar a visita** 
* E, finalmente, se o visitante estiver com status `FINALIZADO`, não vamos exibir botões

### Exibição condicional de botões para autorizar entrada e finalizar visita

Para fazer isso, vamos utilizar a tag `{% if %}` para verificar o status do visitante e renderizar um botão por vez. Primeiro, vamos criar a instrução `if` para verificar se o status é `AGUARDANDO` e renderizar o botão para autorizar a entrada do visitante.

Utilizando a tag `{% if %}` vamos definir a condição `visitante.status == "AGUARDANDO"` para que o botão apareça. Isto é, o HTML referente ao botão só será renderizado no template caso o status do visitante seja `AGUARDANDO`. Nosso código ficará assim:

```markup
{% if visitante.status == "AGUARDANDO" %}
    <a href="#" class="btn btn-success btn-icon-split btn-sm" data-toggle="modal" data-target="#modal1">
        <span class="text">Autorizar entrada</span>
                
        <span class="icon text-white-50">
            <i class="fas fa-user-check"></i>
        </span>
    </a>
{% endif %}
```

Abaixo do código que escrevemos, vamos criar uma outra condição com a tag `{% if %}`. Dessa vez, queremos verificar se o status é o `EM_VISITA`. O código completo ficará assim:

```markup
<div class="d-sm-flex align-items-center justify-content-between mb-4">
    <h1 class="h3 mb-0 text-gray-800">{{ nome_pagina }}</h1>
        
    <div class="">
        {% if visitante.status == "AGUARDANDO" %}
            <a href="#" class="btn btn-success btn-icon-split btn-sm" data-toggle="modal" data-target="#modal1">
                <span class="text">Autorizar entrada</span>
                    
                <span class="icon text-white-50">
                    <i class="fas fa-user-check"></i>
                </span>
            </a>
        {% endif %}

        {% if visitante.status == "EM_VISITA" %}
            <a href="#" class="btn btn-warning btn-icon-split btn-sm" data-toggle="modal" data-target="#modal2">
                <span class="text">Finalizar visita</span>
                    
                <span class="icon text-white-50">
                    <i class="fas fa-door-open"></i>
                </span>
            </a>
        {% endif %}
    </div>
</div>
```

Se você acessar a página de informação de algum visitante, notará que os botões não estão aparecendo juntos mais. Além disso, se você for na página de informação de um visitante que já deixou as dependências do condomínio, isto é, finalizou sua visita, notará que nenhum botão é exibido. 

Dessa forma conseguimos evitar que operações desnecessárias sejam realizadas e que as informações do nosso banco de dados sejam alteradas de forma indevida.

## Bloqueando o acesso à URL por métodos diferentes do POST

Ao contrário das outras funções que escrevemos, a função `finalizar_visita` não poderá ser acessada através do método `GET`. O método `GET` é utilizado por uma requisição sempre que precisamos buscar informações em um servidor, como é o caso nas outras funções \(estamos buscando o template e todo o contexto relacionado a ele antes de enviar informações para o usuário\). Se você notar as funções `registrar_visitante` e `informacoes_visitante`, vai perceber que definimos algumas variáveis fora da instrução `if` que verifica se o método utilizado é o `POST`. Isso porque precisamos dessas variáveis quando o usuário acessa a página, como é o caso do formulário que deverá ser exibido mesmo que uma requisição `POST` não seja enviada.

Para garantir que nossa view possa ser acessada somente pelo método `POST`, vamos utilizar a classe `HttpResponseNotAllowed` para nos ajudar. Ela é quem vai cuidar de toda parte de bloquear o acesso via método `GET` e retornar uma mensagem para o usuário quando isso ocorrer. Antes de tudo, precisamos importá-la em nosso arquivo `views.py` do aplicativo visitantes:

```python
from django.contrib import messages
from django.shortcuts import (
    render, redirect, get_object_or_404
)

from django.http import HttpResponseNotAllowed

from visitantes.models import Visitante
from visitantes.forms import (
    VisitanteForm, AutorizaVisitanteForm
)

from datetime import datetime

# código abaixo omitido
```

Feito isso, tudo que precisamos fazer é utilizar a instrução `else` e retornar a classe `HttpResponseNotAllowed` passando uma lista com os métodos permitidos e uma mensagem a ser exibida caso o método utilizado pela requisição seja diferente. Nosso código ficará assim:

```python
def finalizar_visita(request, id):

    if request.method == "POST":
        visitante = get_object_or_404(Visitante, id=id)

        visitante.status = "FINALIZADO"
        visitante.horario_saida = datetime.now()

        visitante.save()

        messages.success(
            request,
            "Visita finalizada com sucesso"
        )

        return redirect("index")

    else:
        return HttpResponseNotAllowed(
            ["POST"],
            "Método não permitido"
        )
```

Com isso, permitimos que a view seja acessada somente pelo método `POST` e que, quando outro método for utilizado, a view vai retornar o código `HTTP 405` e exibir a mensagem "Método não permitido".

Se você quiser, teste em seu navegador: [http://127.0.0.1:8000/visitantes/4/finalizar-visita/](http://127.0.0.1:8000/visitantes/4/finalizar-visita/).


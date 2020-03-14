# Capítulo 08 - dev

## Criando tela para exibir informações de visitante

Seguindo as especificações que recebemos do cliente, sabemos que existe a necessidade de visualização das informações do visitante na dashboard. Sendo assim, é necessário que a gente trabalhe para que seja possível buscar as informações de cada visitante registrado e exibir essas informações em um template à parte, afim de mostrar mais detalhes de cada visitante.

Se você observar a tabela que lista os visitantes recentes na página inicial da dashboard, vai notar que existe um link para ver informações detalhadas de cada visitante. O que vamos é desenvolver a view que deverá buscar o visitante separadamente e exibir as informações do mesmo.

## Criando a view

Como já sabemos, começaremos a trabalhar na funcionalidade pela função de view, no arquivo `views.py`. Abaixo da função `registrar_visitante()`, vamos criar a função `informacoes_visitante()`.

Essa função de view terá uma pequena diferença com relação à função `registrar_visitante()`, desta vez, além do argumento `request`, vamos também receber um argumento de nome `id`, que representará o `id` do visitante a ser buscado em nosso banco de dados. É assim que vamos identificar qual visitante buscar.

Para adiantar um pouco, já vamos criar a view e deixar algumas coisas prontas, como a variável `context` e o retorno renderizando o template `informacoes_visitante.html`, que ainda vamos criar. Por hora, nossa função `informacoes_visitante()` ficará assim:

```python
# código acima omitido

def informacoes_visitante(request, id):
    
    context = {
        "nome_pagina": "Informações de visitante",
    }
    
    return render(request, "informacoes_visitante.html", context)
```

### Conhecendo o atalho get\_model\_or\_404

Agora que nossa view está escrita e recebe um `id`, precisamos buscar o visitante em nosso banco de dados utilizando esse `id`. Existem várias formas de fazer isso, mas vamos utilizar o atalho `get_model_or_404()` do Django. Para utilizá-lo, temos que passar uma classe modelo a ser utilizada na busca e o parâmetro pelo qual queremos buscar \(em nosso caso, utilizaremos o `id` e, por isso, vamos passá-lo como segundo argumento da função `get_model_or_404()`\).

Antes de tudo, claro, vamos importar a função `get_model_or_404()` em nossa view juntamente com a classe modelo `Visitante`, pois precisaremos dela. A função `get_model_or_404()` está localizada no mesmo pacote que as funções `render` e `redirect`, desta forma, vamos alterar as primeiras linhas do nosso código para o seguinte:

```python
from django.contrib import messages
from django.shortcuts import (
    render, redirect, get_object_or_404
)

from visitantes.models import Visitante
from visitantes.forms import VisitanteForm

# código abaixo omitido
```

Conforme vimos, para utilizar a função `get_object_or_404()`, precisamos passar a classe modelo e o atributo a ser utilizado para busca. Vamos passar a classe `Visitante` e dizer que vamos buscar o visitante pelo `id` e que o `id` é igual à variável que estamos recebendo como argumento da função `informacoes_visitante()`. Nosso código ficará assim:

```python
def informacoes_visitante(request, id):
    
    visitante = get_object_or_404(Visitante, id=id)
    
    context = {
        "nome_pagina": "Informações de visitante",
        "visitante": visitante
    }
    
    return render(request, "informacoes_visitante.html", context)
```

Não vamos nos esquecer de passar a variável `visitante` no contexto, para que possamos acessá-la nos templates!

## Criando URL para acessar informações de visitante

A essa altura você já deve ter percebido que precisamos mapear a nova view em uma URL para conseguirmos acessá-la através do navegador. Dessa forma, vamos trabalhar no arquivo `urls.py` do nosso projeto e criar a URL de nome `informacoes_visitante`.

Essa URL vai ser um pouco diferente das que criamos até agora pois, conforme vimos na função, precisamos de um `id` como argumento que será o identificador do visitante para busca no banco de dados. Podemos passar o `id` diretamente no endereço da URL acessada. Por exemplo, se queremos buscar as informações do visitante de `id=1`, acessamos a URL [http://127.0.0.1:8000/visitantes/1/](http://127.0.0.1:8000/visitantes/1/).

O primeiro passo nós já fizemos, que é receber o argumento na função de view que será mapeada na URL. Agora, temos que utilizar a sintaxe `<int:id>` na URL. Desta forma, estamos dizendo para o Django que precisamos receber uma variável do tipo `int` e ela receberá o nome de `id`. Nossa URL ficará assim:

```python
from django.urls import path
from django.contrib import admin

import usuarios.views
import visitantes.views

urlpatterns = [
    # codigo acima omitido...
    
    path(
        "registrar-visitante",
        visitantes.views.registrar_visitante,
        name="registrar_visitante",
    )
    
    path(
        "visitantes/<int:id>/",
        visitantes.views.informacoes_visitante,
        name="informacoes_visitante",
    )
]
```

## Criando template

Com nossa view pronta, agora precisamos criar o arquivo `informacoes_visitante.html` e colocá-lo na pasta **templates** do nosso projeto. Mais uma vez, você pode fazer download do template clicando no link abaixo:

{% file src="../.gitbook/assets/informacoes\_visitante.html.zip" caption="Iniciar o download" %}

Após o download, coloque o arquivo na pasta **templates** do projeto pois ainda precisamos alterar algumas coisas nele.

Ao abrir o arquivo, você vai perceber que as informações estão definidas diretamente no template. Para tornar o template dinâmico e funcional, vamos alterar os valores do atributo `value` dos elementos `field` do HTML. A estrutura é bem parecida com a que utilizamos nos formulários, com a diferença que vamos renderizar todos os campos manualmente para que possamos personolizar mais a estrutura do HTML.

Como nós passamos a variável visitante no contexto, podemos acessá-la diretamente utilizando a sintaxe de dupla chave. continuar

Feito isso, vamos abrir o navegador e acessar o endereço [http://127.0.0.1:8000/visitantes/1/](http://127.0.0.1:8000/visitantes/1/). Você deverá visualizar as informações do primeiro visitante que registramos no banco de dados.

## Utilizando o Django para renderizar nossas URLs

Para acessar as informações de cada visitante, precisamos acessar a URL `http://127.0.0.1/visitantes/{id}/`, onde `{id}` será um valor diferente para cada visitante.

{% hint style="info" %}
Quando criamos nosso modelo de visitante, não criamos o atributo `id`, mas o Django faz isso por nós para que possamos utilizá-lo como `primary_key`. Além disso, o atributo `id` deverá ser único e, para cada novo visitante registrado, o valor será aumentado em um. Sendo assim, não existirá dois visitante de mesmo `id`.
{% endhint %}

Com isso em mente, você deve estar se perguntando: como vamos fazer para renderizar a URL de informações de visitante de forma automática?

Para nossa sorte, o pessoal responsável pelo Django já pensou em tudo por nós. Dentre as tags de template, existe a tag `{% url %}`. Ela tem a função de renderizar as URLs do nosso projeto de forma automática, bastando que a gente passe apenas o nome da URL da ser renderizado \(sim, é exatamente o valor que definimos para o argumento `name` na definição da URL no arquivos `urls.py`\).

Vamos abrir o arquivo `index.html` e utilizar a tag `{% url %}` para renderizar a URL que irá nos direcionar para o template de informações de cada visitante. Na tabela que exibe as informações dos visitantes recentes existe um link que exibe o texto "Ver detalhes". Vamos alterar o valor de `href` do elemento `<a>` de `#` para `{% url 'informacoes_visitante' id=visitante.id %}`. Primeiro passamos nome da URL e depois podemos passar argumentos necessários que, para esse caso, é somente a `id`. Note também que, dentro do loop, o `id` de cada visitante é acessado da mesma forma que os outros atributos. O loop ficará assim:

```python
{% for visitante in todos_visitantes %}
    <td>{{ visitante.nome_completo }}</td>
    <td>{{ visitante.cpf }}</td>
    <td>{{ visitante.horario_chegada }}</td>
    <td>{{ visitante.horario_autorizacao }}</td>
    <td>{{ visitante.morador_resposavel }}</td>
    <td>
        <a href="{% url 'informacoes_visitante' id=visitante.id %}">
            Ver detalhes
        </a>
    </td>
{% endfor %}
```

Para facilitar o acesso à URL de registro de visitantes, vamos inserir um botão ao lado do nome da página que deverá nos direcionar para a página [http://127.0.0.1:8000/registrar-visitante/](http://127.0.0.1:8000/registrar-vistante/). Abaixo do elemento `<h1 class="h3 mb-0 text-gray-800">{{ nome_pagina }}</h1>` vamos inserir o seguinte trecho de código:

```python
<a href="{% url 'registrar_visitante' %}" class="btn btn-primary btn-icon-split btn-sm">
    <span class="text">Registrar visitante</span>

    <span class="icon text-white-50">
        <i class="fas fa-user-plus"></i>
    </span>
</a>
```

Como nossa URL `registrar_visitante` não recebe argumentos, passamos para a tag apenas o nome da mesma.

## Criando métodos personalizados para exibir informações do Visitante

* Exibindo atributos somente quando preenchidos
  * Criando método get\_horario\_autorizacao
  * Criando método get\_autorizado\_por
  * Criando método get\_horario\_saida

### Utilizando métodos personalizados no template

Com nossos métodos peronslizados criados, temos que 


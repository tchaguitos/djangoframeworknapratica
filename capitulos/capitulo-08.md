# Capítulo 08

## Criando tela para exibir informações de visitante

Seguindo as especificações que recebemos do cliente, sabemos que existe a necessidade de visualização das informações do visitante na dashboard. Sendo assim, é necessário que a gente trabalhe para que seja possível buscar as informações de cada visitante registrado e exibir essas informações em um template à parte, afim de mostrar mais detalhes de cada visitante.

Se você observar a tabela que lista os visitantes recentes na página inicial da dashboard, vai notar que existe um link para ver informações detalhadas de cada visitante. O que vamos fazer é desenvolver a view que vai buscar a informação de um visitante por vez e exibir essas informações de forma estrutura em um template HTML.

## Criando a view

Como já sabemos, começaremos a trabalhar na funcionalidade pela função de view, no arquivo `views.py`. Abaixo da função `registrar_visitante()`, vamos criar a função `informacoes_visitante()`.

Essa função de view terá uma pequena diferença com relação à função `registrar_visitante()`, desta vez, além do argumento `request`, vamos também receber um argumento de nome `id`, que representará o `id` do visitante a ser buscado em nosso banco de dados. É assim que vamos identificar qual visitante devemos buscar.

Já vamos aproveitar para criar a view e deixar algumas coisas prontas, como a variável `context` e o retorno renderizando o template `informacoes_visitante.html`, que ainda vamos criar. Por hora, nossa função `informacoes_visitante()` ficará assim:

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

Antes de tudo, claro, vamos importar a função `get_model_or_404()` em nossa view juntamente com a classe modelo `Visitante`, pois também precisaremos dela. A função `get_model_or_404()` está localizada no mesmo pacote que as funções `render` e `redirect`, desta forma, vamos alterar as primeiras linhas do nosso código para o seguinte:

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

Não vamos nos esquecer de passar a variável `visitante` no contexto, para que possamos acessá-la nos templates.

## Criando URL para acessar informações de visitante

A essa altura você já deve ter percebido que precisamos mapear a nova view em uma URL para que a gente possa acessá-la através do navegador. Sendo assim, vamos trabalhar no arquivo `urls.py` do nosso projeto e criar a URL de nome `informacoes_visitante`.

Essa URL vai ser um pouco diferente das que criamos até agora. Conforme visto, precisamos passar um `id` como argumento para a função que será o identificador do visitante a ser buscado no banco de dados. Isso pode ser feito através da URL que será acessada, pois podemos passar o `id` diretamente no endereço da URL. Por exemplo, se quisermos buscar as informações do visitante de `id=1`, podemos acessar a URL [http://127.0.0.1:8000/visitantes/1/](http://127.0.0.1:8000/visitantes/1/).

O primeiro passo nós já fizemos, que é receber o argumento na função de view que será mapeada na URL. Agora temos que criar a URL no arquivos `urls.py` e utilizar a sintaxe `<int:id>` para indicar que precisamos receber uma variável do tipo `int` e de nome `id`. Nossa URL ficará assim:

```python
from django.urls import path
from django.contrib import admin

from usuarios.views import index

from visitantes.views import (
    registrar_visitante, informacoes_visitante
)

urlpatterns = [
    # codigo acima omitido...
    
    path(
        "registrar-visitante/",
        registrar_visitante,
        name="registrar_visitante",
    ),

    path(
        "visitantes/<int:id>/",
        informacoes_visitante,
        name="informacoes_visitante",
    )
]
```

## Criando template para exibir informações de visitante

Com nossa view pronta, agora precisamos criar o arquivo `informacoes_visitante.html` na pasta **templates** do nosso projeto. Mais uma vez, você pode fazer download do template clicando no link abaixo:

{% file src="../.gitbook/assets/informacoes\_visitante.html.zip" caption="Iniciar o download" %}

Após o download, coloque o arquivo na pasta **templates** do projeto e abra em seu editor de texto, pois ainda vamos alterar algumas coisas nele.

Ao abrir o arquivo, você vai perceber que as informações estão definidas diretamente no template. Para tornar o template dinâmico e funcional, vamos alterar os valores do atributo `value` dos elementos `field` do nosso HTML. A estrutura é bem parecida com a que utilizamos nos formulários, com a diferença que vamos renderizar todos os campos manualmente para que possamos personalizar melhor a estrutura do nosso template.

Como passamos a variável `visitante` no contexto, podemos acessá-la diretamente utilizando a sintaxe `{{ visitante.nome_do_atributo }}`. Vamos alterar os valores estáticos para a sintaxe de template do Django e tornar nosso template dinâmico. Se você ficar na dúvida sobre qual atributo deve exibir, o elemento  `<label>` pode te ajudar!

Antes que a gente esqueça, vamos alterar também o texto que exibe o porteiro responsável pelo registro e o horário que o visitante foi registrado. Para isso, vamos alterar o texto de "Visitante registrado em 15/05/2018 por Walter White" para "Visitante registrado em `{{ visitante.horario_chegada }}` por `{{ visitante.registrado_por }}`". O template ficará assim:

```markup
<div class="card-body">
    <h4 class="mb-3 text-primary">
        Informações gerais
    </h4>

    <form>
        <div class="form-row">
            <div class="form-group col-md-6">
                <label>Horário de chegada</label>
                <input type="text" class="form-control" value="{{ visitante.horario_chegada }}" disabled>
            </div>

            <div class="form-group col-md-6">
                <label>Número da casa a ser visitada</label>
                <input type="text" class="form-control" value="{{ visitante.numero_casa }}" disabled>
            </div>
        </div>

        <div class="form-row">
            <div class="form-group col-md-4">
                <label>Horário de autorização de entrada</label>
                <input type="text" class="form-control" value="{{ visitante.horario_autorizacao }}" disabled>
            </div>

            <div class="form-group col-md-4">
                <label>Entrada autorizada por</label>
                <input type="text" class="form-control" value="{{ visitante.morador_responsavel }}" disabled>
            </div>

            <div class="form-group col-md-4">
                <label>Horário de saída</label>
                <input type="text" class="form-control" value="{{ visitante.horario_saida }}" disabled>
            </div>
        </div>
    </form>

    <h4 class="mb-3 mt-4 text-primary">
        Informações pessoais
    </h4>
    
    <form>
        <div class="form-row">
            <div class="form-group col-md-6">
                <label>Nome completo</label>
                <input type="text" class="form-control" value="{{ visitante.nome_completo }}" disabled>
            </div>

            <div class="form-group col-md-6">
                <label>CPF</label>
                <input type="text" class="form-control" value="{{ visitante.cpf }}" disabled>
            </div>
        </div>

        <div class="form-row">
            <div class="form-group col-md-6">
                <label>Data de nascimento</label>
                <input type="text" class="form-control" value="{{ visitante.data_nascimento }}" disabled>
            </div>

            <div class="form-group col-md-6">
                <label>Placa do veículo</label>
                <input type="text" class="form-control" value="{{ visitante.placa_veiculo }}" disabled>
            </div>
        </div>
    </form>
            
    <p class="mr-2 mt-3 mb-4 text-right">
        <small>
            Visitante registrado em {{ visitante.horario_chegada }} por {{ visitante.registrado_por }}
        </small>
    </p>

    <div class="mr-1 text-right">
        <a href="#" class="btn btn-secondary text-white" type="button">
            <span class="text">Voltar</span>
        </a>
    </div>
</div>
```

Feito isso, vamos abrir o navegador e acessar o endereço [http://127.0.0.1:8000/visitantes/1/](http://127.0.0.1:8000/visitantes/1/). Você deverá visualizar as informações do primeiro visitante que registramos no banco de dados.

## Criando métodos personalizados para exibir informações do Visitante

Conforme exibimos os campos, você deve ter observado que alguns deles ainda não estão preenchidos no banco de dados e, por isso, exibem um valor em branco ou uma informação pouco clara do que, de fato, representa \(`None`\). Para melhorar a exibição destes campos e, consequentemente, a melhorar a usabilidade da nossa dashboard, criaremos métodos personalizados nas classes modelo para que possamos exibir uma informação útil e clara, mesmo quando o campo não está preenchido.

Métodos são funções que existem dentro das classes e podem definir comportamentos para os objetos. Em nosso caso, criaremos métodos que alteram a forma com que as informações são exibidas para o usuário. Se, por exemplo, a entrada do morador ainda não tiver sido autorizada, podemos exibir algo como "Visitante aguardando autorização" nos campos `horario_autorizacao` e `morador_responsavel`. O mesmo vale para o campo `horario_saida`, que só será preenchido no momento que a visita for finalizada.

### Criando método para exibir horário de saída

Vamos criar métodos que vão substituir a exibição de alguns atributos, começando pelo horário de saída. Antes de tudo, você precisa saber que para criar um método dentro de uma classe, tudo que precisamos fazer é criar uma função dentro dessa classe que receberá o argumento `self`. Esse argumento nos possibilita acessar as propriedades da própria classe.

Os métodos que buscam informações, em geral, recebem o nome de **getters** e mantemos sempre a chave "get" no início de seus nomes. Abaixo do atributo `registrado_por`, vamos escrever o método `get_horario_saida()`. Esse método, antes de tudo, precisa verificar se o atributo `horario_saida` está preenchido e, caso não esteja, retornar o texto "Horário de saída não registrado". Para fazer isso, vamos utilizar a estrutura condicional `if`. O método ficará assim:

```python
# codigo acima omitido
def get_horario_saida(self):
    if self.horario_saida:
        return self.horario_saida

    return "Horário de saída não registrado"

class Meta:
    verbose_name = "Visitante"
    verbose_name_plural = "Visitantes"
    db_table = "visitante"

    def __str__(self):
        return self.nome_completo
```

### Criando métodos para exibir horário de autorização de entrada e morador responsável por autorizar a entrada

Faremos o mesmo para os atributos `horario_autorizacao` e `morador_responsavel`, que serão exibidos somente se existir um valor a ser exibido. Caso contrário, vamos exibir um texto padrão. Vamos começar escrevendo o método `get_horario_autorizacao()`, que será bem parecido com o método `get_horario_saida()`.  O método `get_horario_autorizacao()` ficará assim:

```python
def get_horario_autorizacao(self):
    if self.horario_autorizacao:
        return self.horario_autorizacao

    return "Visitante aguardando autorização"
```

Para o método `get_morador_responsavel()` vamos fazer bem parecido. Nosso método ficará assim:

```python
def get_morador_responsavel(self):
    if self.morador_responsavel:
        return self.morador_responsavel

    return "Visitante aguardando autorização"
```

### Criando método para exibir placa do veículo utilizado na visita

O método `get_veiculo()` será parecido com os outros, mas também terá um outro texto padrão:

```python
def get_placa_veiculo(self):
    if self.placa_veiculo:
        return self.placa_veiculo

    return "Veículo não registrado"
```

### Utilizando métodos personalizados nos templates

Com nossos métodos criados, temos que alterar o template `informacoes_visitante.html` para que exiba os métodos os invés dos atributos. A sintaxe para exibição de métodos nos templates é bem parecida com a que utilizamos para os atributos, inclusive.

Onde temos os atributos `horario_autorizacao`, `morador_responsavel`, `horario_saida` e `placa_veiculo`, vamos alterar para métodos criados. Ou seja, ao invés de `{{ visitante.horario_autorizacao }}`, utilizaremos `{{ visitante.get_horario_autorizacao }}`. O template ficará assim:

```markup
<div class="card-body">
    <h4 class="mb-3 text-primary">
        Informações gerais
    </h4>

    <form>
        <div class="form-row">
            <div class="form-group col-md-6">
                <label>Horário de chegada</label>
                <input type="text" class="form-control" value="{{ visitante.horario_chegada }}" disabled>
            </div>

            <div class="form-group col-md-6">
                <label>Número da casa a ser visitada</label>
                <input type="text" class="form-control" value="{{ visitante.numero_casa }}" disabled>
            </div>
        </div>

        <div class="form-row">
            <div class="form-group col-md-4">
                <label>Horário de autorização de entrada</label>
                <input type="text" class="form-control" value="{{ visitante.get_horario_autorizacao }}" disabled>
            </div>

            <div class="form-group col-md-4">
                <label>Entrada autorizada por</label>
                <input type="text" class="form-control" value="{{ visitante.get_morador_responsavel }}" disabled>
            </div>

            <div class="form-group col-md-4">
                <label>Horário de saída</label>
                <input type="text" class="form-control" value="{{ visitante.get_horario_saida }}" disabled>
            </div>
        </div>
    </form>

    <h4 class="mb-3 mt-4 text-primary">
        Informações pessoais
    </h4>
    
    <form>
        <div class="form-row">
            <div class="form-group col-md-6">
                <label>Nome completo</label>
                <input type="text" class="form-control" value="{{ visitante.nome_completo }}" disabled>
            </div>

            <div class="form-group col-md-6">
                <label>CPF</label>
                <input type="text" class="form-control" value="{{ visitante.cpf }}" disabled>
            </div>
        </div>

        <div class="form-row">
            <div class="form-group col-md-6">
                <label>Data de nascimento</label>
                <input type="text" class="form-control" value="{{ visitante.data_nascimento }}" disabled>
            </div>

            <div class="form-group col-md-6">
                <label>Placa do veículo</label>
                <input type="text" class="form-control" value="{{ visitante.get_placa_veiculo }}" disabled>
            </div>
        </div>
    </form>
            
    <p class="mr-2 mt-3 mb-4 text-right">
        <small>
            Visitante registrado em {{ visitante.horario_chegada }} por {{ visitante.registrado_por }}
        </small>
    </p>

    <div class="mr-1 text-right">
        <a href="#" class="btn btn-secondary text-white" type="button">
            <span class="text">Voltar</span>
        </a>
    </div>
</div>
```

Não podemos esquecer do template `index.html`, onde também vamos utilizar os métodos `get_horario_autorizacao` e `get_morador_responsavel`. O trecho de código ficará assim:

```markup
<tbody>
    {% for visitante in todos_visitantes %}
        <tr>
            <td>{{ visitante.nome_completo }}</td>
            <td>{{ visitante.cpf }}</td>
            <td>{{ visitante.horario_chegada }}</td>
            <td>{{ visitante.get_horario_autorizacao }}</td>
            <td>{{ visitante.get_morador_responsavel }}</td>
            <td>
                <a href="{% url 'informacoes_visitante' id=visitante.id %}">
                    Ver informações
                </a>
            </td>
        </tr>
    {% endfor %}
</tbody>
```

Você pode criar os métodos que quiser e exibir as informações conforme precisar. Existem diversas possibilidades e aplicações, então sinta-se livre para explorar essas possibilidades!

## Utilizando o Django para renderizar nossas URLs

Para acessar as informações de cada visitante, precisamos acessar a URL `http://127.0.0.1/visitantes/{id}/`, onde o `{id}` será um valor diferente para cada visitante. Até agora fizemos isso manualmente, mas você deve estar se perguntando: como vamos fazer para renderizar uma URL diferente para cada de visitante de forma automática?

{% hint style="info" %}
Quando criamos nosso modelo de visitante, não criamos o atributo `id`, mas o Django faz isso por nós para que possamos utilizá-lo como `primary_key`. Além disso, o atributo `id` deverá ser único e, para cada novo visitante registrado, o valor será aumentado em um. Sendo assim, não existirá dois visitante de mesmo `id`.
{% endhint %}

Para nossa sorte, o pessoal responsável pelo Django já pensou em tudo por nós. Dentre as tags de template, existe a tag `{% url %}`. Ela tem a função de renderizar as URLs do nosso projeto de forma automática, bastando que a gente passe apenas o nome da URL da ser renderizada \(sim, é exatamente o valor que definimos para o argumento `name` na definição da URL no arquivos `urls.py`\).

Vamos abrir o arquivo `index.html` e utilizar a tag `{% url %}` para renderizar a URL que irá nos direcionar para o template de informações de cada visitante. Na tabela que exibe as informações dos visitantes recentes existe um link que exibe o texto "Ver detalhes". Vamos alterar o valor de `href` do elemento `<a>` de `#` para `{% url 'informacoes_visitante' id=visitante.id %}`. Primeiro passamos nome da URL e depois podemos passar argumentos necessários que, para esse caso, é somente a `id`. Note também que, dentro do loop, o `id` de cada visitante é acessado da mesma forma que os outros atributos. O loop ficará assim:

```markup
{% for visitante in todos_visitantes %}
    <td>{{ visitante.nome_completo }}</td>
    <td>{{ visitante.cpf }}</td>
    <td>{{ visitante.horario_chegada }}</td>
    <td>{{ visitante.horario_autorizacao }}</td>
    <td>{{ visitante.morador_responsavel }}</td>
    <td>
        <a href="{% url 'informacoes_visitante' id=visitante.id %}">
            Ver detalhes
        </a>
    </td>
{% endfor %}
```

Para facilitar o acesso à URL de registro de visitantes, vamos inserir um botão ao lado do nome da página que deverá nos direcionar para a página [http://127.0.0.1:8000/registrar-visitante/](http://127.0.0.1:8000/registrar-vistante/). Abaixo do elemento `<h1 class="h3 mb-0 text-gray-800">{{ nome_pagina }}</h1>` vamos inserir o seguinte trecho de código:

```markup
<a href="{% url 'registrar_visitante' %}" class="btn btn-primary btn-icon-split btn-sm">
    <span class="text">Registrar visitante</span>

    <span class="icon text-white-50">
        <i class="fas fa-user-plus"></i>
    </span>
</a>
```

Como nossa URL `registrar_visitante` não recebe argumentos, passamos para a tag apenas o nome da mesma.


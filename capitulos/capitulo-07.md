# Capítulo 07

## Trabalhando com formulários no Django

Manipular formulários não é uma tarefa tão fácil. Se observarmos o Admin do Django, podemos notar que existem diversos tipos de dados e maneiras diferentes de tratar e renderizar esses dados. Além disso, existe a estrutura HTML do formulário a ser renderizada no template, esse formulário deve validar as informações que são enviadas pelo usuário, salvar as informações ou exibir uma mensagem para o usuário caso os dados estejam inválidos, etc. Para simplificar nosso trabalho, o Django fornece ferramentas para automatizar e simplificar esse processo, garantindo também segurança para implementar as funcionalidades necessárias.

Um formulário pode ser definido como um conjunto de elementos dentro do elemento HTML `<form>` que permitem que o usuário insira textos, números, escolha opções e, ao fim, envie essas informações de volta para o servidor. No contexto da nossa aplicação web, um formulário pode significar também o formulário que a classe `Form` do Django nos disponibiliza, que é quem faz toda mágica por nós. Da mesma maneira que uma classe `Model` descreve toda estrutura lógica de um objeto, seu comportamento e a maneira como suas partes são representadas para nós, uma classe `Form` descreve um formulário e determina como ele funciona e se parece.

## Criando formulário para registro de visitante

Assim como outras camadas importantes da arquitetura do nosso projeto, os formulários também devem ter um arquivo próprio para eles, mas que, neste caso, precisamos criar: o arquivo `forms.py`. Vamos abrir a pasta do nosso aplicativo **visitantes** e criar o arquivo `forms.py`. Após criarmos o arquivo, vamos abri-lo e importar o pacote forms do django. Ficará assim:

```python
from django import forms
```

De forma semelhante aos campos de uma classe `Model` que são mapeados para os campos do banco de dados, os campos de uma classe `Form` são mapeados para elementos HTML. É exatamente assim que toda a mágica do Admin do Django funciona: mapeando os campos da sua classe `Model`, criando classes `Form` e renderizando esses campos como elementos HTML. Muito bacana, não?

Para quando queremos mapear automaticamente os campos de uma classe `Model`, o Django nos permite utilizar a classe `ModelForm.` Tudo que precisamos fazer é definir uma subclasse de `ModelForm` e depois identificarmos a classe `Model` a ser utilizada. Sendo assim, vamos também importar a classe **Visitante** e depois definir uma subclasse de `forms.ModelForm` de nome `VisitanteForm`. O arquivo `forms.py` ficará assim:

```python
from django import forms
from visitantes.models import Visitante

class VisitanteForm(forms.ModelForm):
    class Meta:
        model = Visitante
        fields = "__all__"
```

Criamos a subclasse de `forms.ModelForm` chamada `VisitanteForm`, que é quem representa nosso formulário e definimos a classe interna `Meta` para explicitarmos qual classe `Model` deve ser utilizada \(`model = Visitante`\) e quais campos devem ser renderizados \(`fields = "__all__"`\). Por hora, vamos utilizar a string  `"__all__"` para renderizarmos todos os campos.

## Renderizando nosso formulário automaticamente

Agora que definimos a classe que vai representar nosso formulário, podemos partir para a segunda etapa, que é renderizar esse formulário diretamente no HTML. Para isso, vamos trabalhar no arquivo `views.py` do aplicativo **visitantes**, começando pela importação do formulário. Após isso, vamos criar uma variável de nome `form` que será igual à uma instância da classe `VisitanteForm` e passá-la dentro da variável `context` da view `registrar_visitante`. O arquivo ficará assim: 

```python
from django.shortcuts import render
from visitantes.forms import VisitanteForm

def registrar_visitante(request):

    form = VisitanteForm()
    
    context = {
        "nome_pagina": "Registrar visitante",
        "form": form,
    }

    return render(request, "registrar_visitante.html", context)
```

Apenas com as alterações realizadas, já podemos trabalhar no template `registrar_visitante.html` para que o formulário seja renderizado de forma automática. Vamos abrir o arquivo `registrar_visitante.html` dentro da pasta **templates** e procurar pelo elemento HTML `<form>`. Substitua todo o conteúdo existente dentro do elemento pela variável `{{ form }}` e acesse [http://127.0.0.1:8000/registrar-visitante/](http://127.0.0.1:8000/registrar-visitante/) em seu navegador. O arquivo `registrar_visitante.html` ficará assim:

```markup
<!-- codigo acima omitido -->
<div class="card-body">
    <h4 class="mb-3 text-primary">
        Formulário para registro de novo visitante
    </h4>

    <div class="container">
        {{ form }}
    </div>
</div>
<!-- codigo abaixo omitido -->
```

{% hint style="success" %}
Definimos no contexto também a variável `nome_pagina`, mas desta vez, como "Registrar visitante". Note como o Django reconhece o valor da variável de acordo com cada view e altera o valor no template `base.html`
{% endhint %}

## Melhorando a exibição do nosso formulário

Quando o assunto é criar formulários, o Django faz esse papel muito bem, além de prover uma funcionalidade segura e estável. Veja bem, com menos de 10 linhas conseguimos criar e renderizar um formulário que se adapta totalmente às necessidades do nosso modelo. Desta forma, é altamente recomendado utilizar os formulários do Django para automatizar nosso trabalho.

O Django faz muito bem o trabalho que se propõe a fazer: preparar e reestruturar os dados para renderização, criar o formulário para receber os dados e ainda processar e validar esses dados, mas quando precisamos renderizar essas informações no formato HTML de modo que fique mais atrativo para o usuário, faltam algumas opções. É aí que entra o **django-widget-tweaks**, um pacote Python muito interessante e útil disponibilizado pela comunidade para nos ajudar na renderização dos nossos formulários.

## Estilizando nosso formulário com django-widget-tweaks

O **django-widget-tweaks** nos ajuda tornando mais fácil o processo de adicionar atributos e classes aos campos de um formulário Django, afim de aplicar classes personalizadas para alterar aparência ou comportamento dos elementos, quando necessário. Como estamos utilizando um tema que utiliza o [Bootstrap](https://getbootstrap.com/) como base, podemos também utilizar suas classes CSS para alterar a aparência dos elementos.

Isso resolve o problema do nosso formulário não estar sendo exibido de maneira atrativa para o usuário. Isso porque o Django renderiza um formulário HTML simples, sem adicionar classes para alterar o estilo dos elementos que compõem esse formulário. Sendo assim, utilizaremos o **django-widget-tweaks** para adicionar a classe `form-control` aos campos do nosso formulário, e assim aplicar as características descritas no arquivo de estilização \(CSS\) do tema utilizado.

### Como instalar

Para instalar o **django-widget-tweaks** utilizaremos nosso já conhecido gerenciador de pacotes: o `pip`. Para instalar vamos utilizar o seguinte comando:

```bash
(env)$ pip install django-widget-tweaks
```

Caso ocorra bem tudo, você terá instalado o **django-widget-tweaks** em seu ambiente virtual. Feito isso, também vamos adicionar o pacote à variável `INSTALLED_APPS` do nosso arquivo de configurações. Para uma melhor organização, vamos criar uma lista separada da lista dos nossos aplicativos.

```python
# código acima omitido

INSTALLED_APPS += [
    "widget_tweaks",
]

INSTALLED_APPS += [
    "usuarios",
    "porteiros",
    "visitantes",
]

# código abaixo omitido
```

{% hint style="info" %}
Utilizamos três variáveis de mesmo nome e as incrementamos pois assim separamos os aplicativos do Django \(primeira\), os pacotes Python instalados \(segunda\) e os aplicativos criados por nós \(terceira\). Lembre-se que é necessário utilizar o operador `+=` quando queremos incrementar os valores existentes na variável
{% endhint %}

### Importando no template

Agora que instalamos e registramos o pacote em nosso arquivo de configurações, temos que utilizar a tag `{% load widget_tweaks %}` sempre que precisarmos utilizar as funcionalidades do pacote num determinado template. Vamos adicionar a tag logo abaixo da primeira linha do template `registrar_visitante.html`. O arquivo ficará assim:

```markup
{% extends "base.html" %}

{% load widget_tweaks %}

<!-- código abaixo omitido -->
```

### Utilizando o render\_field

Existem duas maneiras que o **django-widget-tweaks** nos permite utilizar suas funcionalidades, mas utilizaremos a tag personalizada `{% render_field %}`, com ela conseguimos descrever nossos campos de forma bem parecida com o HTML5.

Vamos abrir o arquivo `registrar_visitante.html` e substituir a variável `{{ form }}` pelo elemento `<form method="post">` abaixo e seu conteúdo. O código ficará assim:

```markup
<!-- codigo acima omitido -->
<div class="card-body">
    <h4 class="mb-3 text-primary">
        Formulário para registro de novo visitante
    </h4>

    <p class="mb-5 ml-1">
        <small>
            O asterisco (*) indica que o campo é obrigatório
        </small>
    </p>
    
    <form method="post">
        <div class="form-row">
            {% csrf_token %}

            {% for field in form %}
                <div class="form-group col-md-12">
                    <label>{{ field.label }} {% if field.field.required %} * {% endif %}</label>
                    {% render_field field placeholder=field.label class="form-control" %}
                </div>
            {% endfor %}
        </div>
        
        <div class="text-right">
            <button class="btn btn-primary" type="submit">
                <span class="text">Registrar visitante</span>
            </button>
        </div>
    </form>
</div>
<!-- codigo abaixo omitido -->
```

Vamos adicionar também um trecho de código HTML com um aviso sinalizando que o asterisco \(\*\) acima dos campos do formulário indica que o campo é obrigatório.

{% hint style="info" %}
A tag `{% csrf_token %}` fornece proteção para nossa aplicação, de modo a impedir que sites mal intencionados enviem requisições para ela. Caso a gente não coloque essa tag dentro dos nossos formulários, o Django não aceitará a requisição enviada e mostrará um erro pois não vai identificar a requisição como segura
{% endhint %}

Logo abaixo da tag `{% csrf_token %}`, estamos utilizando novamente a tag `{% for %}` para realizar um loop, mas desta vez na variável `form`. Quando realizamos um loop em nosso formulário, conseguimos acessar seus campos, e é exatamente o que precisamos fazer: executar um loop e acessar as informações de cada campo para que possamos passá-las para a tag `{% render_field %}` fazer o trabalho de renderização destes campos.

{% hint style="info" %}
Aqui temos uma novidade, a utilização da tag `{% if %}`. Uma estrutura condicional que pode ser utilizada em templates. O que estiver dentro dela só será exibido caso o resultado da expressão seja verdadeiro. Ou seja, quando existem mensagens e a variável `messages` está definida, exibimos o trecho HTML
{% endhint %}

Para cada campo \(_variável field_\) em nosso formulário, criamos a estrutura padrão para campos de um formulário do nosso tema. Acessamos também a propriedade `label`  para exibir o nome e o _placeholder_ do `input` e passamos a variável que representa o campo para a tag `{% render_field %}`. Veja como ficará estrutura de cada campo:

```markup
<div class="form-group col-md-12">
    <label>{{ field.label }} {% if field.field.required %} * {% endif %}</label>
    {% render_field field placeholder=field.label class="form-control" %}
</div>
```

Note que definimos também os atributos `placeholder=field.label` e `class="form-control"`, além de verificarmos se o campo é obrigatório e, caso seja, colocamos um asterisco \(\*\) ao lado do nome do campo. Acesse a página e veja na prática como o layout do nosso formulário melhorou e muito!


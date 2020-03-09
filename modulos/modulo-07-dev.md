# Módulo 07 - dev

## Trabalhando com formulários no Django

Manipular formulários não é uma tarefa tão fácil. Se observarmos o Admin do Django, podemos notar que existem diversos tipos de dados e maneiras diferentes de tratar e renderizar esses dados. Além disso, existe a estrutura HTML do formulário a ser exibida no template, esse formulário deve validar as informações que são enviadas pelo usuário, salvar as informações ou exibir uma mensagem para o usuário caso os dados estejam inválidos, etc. Para simplificar nosso trabalho, o Django fornece ferramentas para automatizar e simplificar esse processo, garantindo também segurança para implementar as funcionalidades necessárias.

Um formulário pode ser definido como um conjunto de elementos dentro do elemento HTML `<form>` que permitem que o usuário insira textos, números, escolha opções e, ao fim, envie essas informações de volta para o servidor. No contexto da nossa aplicação web, um formulário pode significar também o formulário que a classe `Form` do Django nos disponibiliza, que é quem faz toda mágica por nós. Da mesma maneira que uma classe `Model` descreve toda estrutura lógica de um objeto, seu comportamento e a maneira como suas partes são representadas para nós, uma classe `Form` descreve um formulário e determina como ele funciona e aparece.

## Criando nosso formulário

Assim como outras camadas importantes da arquitetura do nosso projeto, os formulários também devem ter um arquivo próprio para eles, mas que, neste caso, precisamos criar: o arquivo `forms.py`. Vamos abrir a pasta do nosso aplicativo **visitantes** e criar o arquivo `forms.py`. Após criarmos o arquivo, vamos abri-lo e importar o módulo forms do django. Ficará assim:

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

Agora que definimos a classe que vai representar nosso formulário, podemos partir para a segunda etapa, que é renderizar esse formulário diratamente no HTML. Para isso, vamos trabalhar no arquivo `views.py` do aplicativos **visitantes**, começando pela importação do formulário. Após isso, vamos criar uma variável de nome `form` que será igual à uma instância da classe `VisitanteForm` e passá-la dentro do `contexto` da view `registrar_visitante`. O arquivo ficará assim: 

```python
from django.shortcuts import render

from visntantes.forms import VisitanteForm

def registrar_visitante(request):

    form = VisitanteForm()
    
    contexto = {
        "form": form,
    }

    return render(request, "registrar_visitante.html", contexto)
```

Apenas com as alterações realizadas, já podemos trabalhar no template `registrar_visitante.html` para que o formulário seja renderizado de forma automática. Vamos abrir o arquivo `registrar_visitante.html` dentro da pasta **templates** e procurar pelo elemento HTML `<form>`. Substituia todo o conteúdo existente dentro do elemento pela variável `{{ form.as_p }}` e acesse [http://127.0.0.1:8000/registrar-vistante/](http://127.0.0.1:8000/registrar-vistante/) em seu navegador. O arquivo `registrar_visitante.html` ficará assim:

```markup
<!-- codigo acima omitido -->
<div class="card-body">
    <div class="container">
        {{ form.as_p }}
    </div>
</div>
<!-- codigo abaixo omitido -->
```

{% hint style="info" %}
O método `as_p` renderiza o formulário como uma série de tags `<p>` onde cada tag é um campo do formulário
{% endhint %}

## Melhorando a exibição do nosso formulário

Quando o assunto é criar formulários, o Django faz esse papel muito bem, além de prover uma funcionalidade segura e estável. Veja bem, com menos de 10 linhas conseguimos criar e renderizar um formulário que se adequa totalmente à necessidades do nosso modelo. Desta forma, é altamente recomendado utilizar os formulários do Django para automatizar nosso trabalho.

O Django faz muito bem o trabalho que se propõe a fazer: preparar e reestruturar os dados para renderização, criar o formulário para receber os dados e ainda processar e validar esses dados, mas quando precisamos renderizar essas informações no formato HTML de modo que fique mais atrativo para o usuário, faltam algumas opções. É aí que entra o **django-widget-tweaks**, um módulo Python muito interessante e útil disponibilizado pela comunidade para nos ajudar na renderização dos nossos formulários.

## Estilizando nosso formulário com django-widget-tweaks

O **django-widget-tweaks** nos ajuda tornando mais fácil o processo de adicionar atributos e classes aos campos HTML, afim de aplicar classes personalizadas para personalizar aparência e comportamento de forma mais simples.

* [https://github.com/jazzband/django-widget-tweaks](https://github.com/jazzband/django-widget-tweaks)
* [https://simpleisbetterthancomplex.com/2015/12/04/package-of-the-week-django-widget-tweaks.html](https://simpleisbetterthancomplex.com/2015/12/04/package-of-the-week-django-widget-tweaks.html)

### Como instalar

* inserindo nas configurações

### Importando no template

* importa tags no template

### Utilizando o render\_fiel

Exibindo apenas campos 

## Preparando view para receber requisição do tipo POST

* falar sobre "request"
* validações do modelForm
* falar sobre redirect e csrf\_token

## Conhecendo um pouco mais dos formulários

* [ ] Falar sobre relacionamento entre FKs
* [ ] Tratar problema de valor nulo para campo "autorizado\_por"

## Exibindo mensagem para o usuário ao cadastrar novo visitante

* [https://github.com/tchaguitos/controle-visitantes/commit/c414e3068e2fffd9272c4e8fee306d287a21219b](https://github.com/tchaguitos/controle-visitantes/commit/c414e3068e2fffd9272c4e8fee306d287a21219b)\)

## Conhecendo o Django messages

* Adicionando uma mensagem
* Alterando o template para exibir as mensagens

## Tratando possíveis erros em nosso formulário

* Alterando template para exibir mensagem de erro em formulário

## Deixando nossas mensagens de erro mais claras 

* error\_messages


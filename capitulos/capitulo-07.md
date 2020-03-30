# Capítulo 07

## Trabalhando com formulários no Django

Manipular formulários não é uma tarefa tão fácil. Se observarmos o Admin do Django, podemos notar que existem diversos tipos de dados e maneiras diferentes de tratar e renderizar esses dados. Além disso, existe a estrutura HTML do formulário a ser exibida no template, esse formulário deve validar as informações que são enviadas pelo usuário, salvar as informações ou exibir uma mensagem para o usuário caso os dados estejam inválidos, etc. Para simplificar nosso trabalho, o Django fornece ferramentas para automatizar e simplificar esse processo, garantindo também segurança para implementar as funcionalidades necessárias.

Um formulário pode ser definido como um conjunto de elementos dentro do elemento HTML `<form>` que permitem que o usuário insira textos, números, escolha opções e, ao fim, envie essas informações de volta para o servidor. No contexto da nossa aplicação web, um formulário pode significar também o formulário que a classe `Form` do Django nos disponibiliza, que é quem faz toda mágica por nós. Da mesma maneira que uma classe `Model` descreve toda estrutura lógica de um objeto, seu comportamento e a maneira como suas partes são representadas para nós, uma classe `Form` descreve um formulário e determina como ele funciona e aparece.

## Criando nosso formulário

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

Agora que definimos a classe que vai representar nosso formulário, podemos partir para a segunda etapa, que é renderizar esse formulário diratamente no HTML. Para isso, vamos trabalhar no arquivo `views.py` do aplicativos **visitantes**, começando pela importação do formulário. Após isso, vamos criar uma variável de nome `form` que será igual à uma instância da classe `VisitanteForm` e passá-la dentro do `contexto` da view `registrar_visitante`. O arquivo ficará assim: 

```python
from django.shortcuts import render
from visitantes.forms import VisitanteForm

def registrar_visitante(request):

    form = VisitanteForm()
    
    contexto = {
        "form": form,
    }

    return render(request, "registrar_visitante.html", contexto)
```

Apenas com as alterações realizadas, já podemos trabalhar no template `registrar_visitante.html` para que o formulário seja renderizado de forma automática. Vamos abrir o arquivo `registrar_visitante.html` dentro da pasta **templates** e procurar pelo elemento HTML `<form>`. Substituia todo o conteúdo existente dentro do elemento pela variável `{{ form }}` e acesse [http://127.0.0.1:8000/registrar-vistante/](http://127.0.0.1:8000/registrar-vistante/) em seu navegador. O arquivo `registrar_visitante.html` ficará assim:

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

## Melhorando a exibição do nosso formulário

Quando o assunto é criar formulários, o Django faz esse papel muito bem, além de prover uma funcionalidade segura e estável. Veja bem, com menos de 10 linhas conseguimos criar e renderizar um formulário que se adequa totalmente à necessidades do nosso modelo. Desta forma, é altamente recomendado utilizar os formulários do Django para automatizar nosso trabalho.

O Django faz muito bem o trabalho que se propõe a fazer: preparar e reestruturar os dados para renderização, criar o formulário para receber os dados e ainda processar e validar esses dados, mas quando precisamos renderizar essas informações no formato HTML de modo que fique mais atrativo para o usuário, faltam algumas opções. É aí que entra o **django-widget-tweaks**, um pacote Python muito interessante e útil disponibilizado pela comunidade para nos ajudar na renderização dos nossos formulários.

## Estilizando nosso formulário com django-widget-tweaks

O **django-widget-tweaks** nos ajuda tornando mais fácil o processo de adicionar atributos e classes aos campos HTML, afim de aplicar classes personalizadas para alterar aparência e comportamento dos elementos de forma mais simples. Como estamos utilizando um tema que utiliza o [Bootstrap](https://getbootstrap.com/) como base, podemos utilizar suas classes CSS para alterar a aparência desses elementos.

Isso resolve o problema do nosso formulário não estar sendo exibido de maneira atrativa para o usuário. Isso porque o Django renderiza um formulário HTML simples, sem adicionar classes para alterar o estilo dos elementos que compõem esse formulário. Sendo assim, utilizaremos o **django-widget-tweaks** para adicionar a classe `form-control` aos campos do nosso formulário, e assim apliciar as características descritas no arquivo de estilização \(CSS\) do tema utilizado.

### Como instalar

Para instalar o **django-widget-tweaks** utilizaremos nosso já conhecido gerenciador de pacotes: o `pip`. Para instalar vamos utilizar o seguinte comando:

```bash
(env)$ pip install django-widget-tweaks
```

Caso ocorra bem tudo, você terá instalado o **django-widget-tweaks** em seu ambiente virtual. Feito isso, também vamos adicionar o pacote à variável `INSTALLED_APPS` do nosso arquivo de configurações. Para organizar melhor, vamos escrever a variável acima da existente e que lista nossos aplicativos:

```bash
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
Utilizamos três variáveis de mesmo nome e a incrementamos pois assim separamos os aplicativos do Django \(primeira\), os pacotes Python instalados \(segunda\) e os aplicativos criados por nós \(terceira\). Lembre-se que é necessário utilizar o operador `+=` quando queremos incrementar os valores existentes na variável
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

Vamos abrir o arquivo `registrar_visitante.html` e substituir o elemento `<div class="container">` pelo `<form method="post">` e seu conteúdo. O código ficará assim:

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
    </form>
</div>
<!-- codigo abaixo omitido -->
```

{% hint style="info" %}
A tag `{% csrf_token %}` fornece proteção para nossa aplicação, de modo a impedir que sites mal intencionados enviem requisições para ela. Caso a gente não coloque essa tag dentro dos nossos formulários, o Django não aceitará a requisição enviada e mostrará um erro. 
{% endhint %}

Logo abaixo da tag `{% csrf_token %}`, estamos utilizando novamente a tag `{% for %}` para realizar um loop, mas desta vez na variável `form`. Quando realizamos um loop em nosso formulário, conseguimos acessar seus campos, e é exatamente o que precisamos fazer: executar um loop e acessar as informações de cada campo para que possamos passá-las para a tag `{% render_field %}` fazer o trabalho de renderização destes campos.

Para cada campo \(_variável field_\) em nosso formulário, criamos a estrutura padrão para campos do formulário HTML, acessamos a propriedade `label`  para exibir o nome e o placeholder do `input` e passamos a variável que representa o campo para a tag `{% render_field %}`. Veja como fica nossa estrutura:

```markup
<div class="form-group col-md-12">
    <label>{{ field.label }} {% if field.field.required %} * {% endif %}</label>
    {% render_field field placeholder=field.label class="form-control" %}
</div>
```

Note que definimos também os atributos `placeholder=field.label` e `class="form-control"`, além de verificarmos se o campo é obrigatório e, caso seja, colocamos um asterisco \(\*\) ao lado do nome do campo. A classe `form-control` que passamos como atributo do campo é quem estiliza e torna a exibição do campo mais atrativa. 

Acesse a página e veja na prática como o layout do nosso formulário melhorou e muito!

## Preparando view para receber requisição do tipo POST

Agora que cuidamos da usabilidade do nosso formulário, podemos seguir com as outras partes da nossa view. Até o momento, apenas criamos a variável que representa o formulário e a passamos no contexto, o que faremos agora é preparar nossa view para receber os dados que serão enviados na requisição.

### Conhecendo o objeto request

Você já deve ter notado que sempre que criamos uma view, precisamos que ela receba a variável `request` como argumento. Isso porque, quando falamos do protocolo que sustenta a web, o HTTP, requisições são o que movimentam toda a estrutura. Quando acessamos uma página web estamos enviando a ela uma requisição do tipo `GET` e ela, assim que possível, nos retornará com o conteúdo da página acessada. Isso tudo acontece em questão de segundos e por baixo dos panos, nos fios que conectam a web. Bacana, não?

A variável `request` é uma representação da requisição que é enviada à view acessada e contém diversas informações como usuário logado, método HTTP utilizado, navegador e sistema operacional, dentre outras. No momento, nos interessa o método da requisição e o corpo que é enviado. 

Para prepararmos a view para receber as informações da requisição, vamos adicionar um `if` abaixo da linha `form = VisitanteForm()` para verificar se o método da requisição é do tipo `POST`. Podemos fazer isso dessa forma:

```python
from django.shortcuts import render
from visitantes.forms import VisitanteForm

def registrar_visitante(request):

    form = VisitanteForm()
    
    if request.method == "POST":
        print("o método é post")
    
    contexto = {
        "form": form,
    }

    return render(request, "registrar_visitante.html", contexto)
```

{% hint style="success" %}
O método `POST` é utilizado sempre que precisamos enviar informações para o servidor. No nosso caso, por exemplo, queremos enviar as informações de um novo visitante a ser registrado e fazemos isso através do formulário HTML
{% endhint %}

Agora que verificamos se o método da requisição enviada é do tipo `POST`, vamos passar o corpo da requisição para o formulário e utilizar o método `is_valid` do formulário para validar as informações. O código ficará o seguinte:

```python
from django.shortcuts import render
from visitantes.forms import VisitanteForm

def registrar_visitante(request):

    form = VisitanteForm()
    
    if request.method == "POST":
        form = VisitanteForm(request.POST)
        
        if form.is_valid():
            form.save()
    
    contexto = {
        "form": form,
    }

    return render(request, "registrar_visitante.html", contexto)
```

Para passar o corpo da requisição para o formulário, basta utilizarmos a propriedade POST do objeto request e passá-lo como argumento ao criarmos a nova instância do formulário, como feito na linha 9 \(`form = VisitanteForm(request.POST)`\). Feito isso, validamos as informações e salvamos o formulário utilizando o método `save`.

## Conhecendo um pouco mais dos formulários

Ao acessarmos a página, podemos notar que todos os campos do modelo estão sendo exibidos, e não é isso que queremos, pois algumas das informações devem ser preenchidas mediante autorização de moradores e outros eventos.

Para especificar os campos que devem ser exibidos e utilizados no formulário, vamos voltar ao arquivo `forms.py` e alterar o atributo `fields` da classe `Meta` do nosso formulário. Abra o arquivo e substituia a string `"__all__"` por uma lista com os nomes dos campos que precisamos exibir. O atributo `fields` ficará assim:

```python
from django import forms
from visitantes.models import Visitante

class VisitanteForm(forms.ModelForm):
    class Meta:
        model = Visitante
        fields = [
            "nome_completo", "cpf", "data_nascimento",
            "numero_casa", "placa_veiculo",
        ]
```

Ao voltar para a página, vamos notar que agora apenas os campos que estão na lista `fields` da classe `Meta` estão sendo exibidos.

### Tratando problema com atributo nulo

Quando criamos a classe modelo `Visitante`, falamos sobre o atributo `registrado_por` ser do tipo `ForeignKey`, um tipo de campo que cria um relacionamento entre as classes `Visitante` e `Porteiro`. Olhando a classe `VisitanteForm`, podemos notar que o atributo não é colocado nos campos do formulário \(`fields`\), mesmo este sendo do tipo obrigatório em nosso modelo. Sendo assim, se tentarmos adicionar um visitante por meio do formulário, o Django apresentará um erro nos informando que o atributo`registrado_por` do modelo não pode ser nulo.

Para resolver o problema, o que vamos fazer é possibilitar que o campo seja preechido de maneira automática. Isto é, o campo receberá o valor referente ao porteiro que está logado na dashboard no momento do cadastro.

Para fazer isso, antes de salvar o formulário vamos definir diretamente um valor para o atributo `registrado_por` na view. Vamos abrir o arquivo `views.py` e criar uma variável para receber o retorno do método `save` do formulário. Esse método aceita também um argumento opicional de nome `commit`, que quando definido como `False`, retorna uma instância do modelo utilizado no formulário que ainda não foi gravada no banco de dados. Isso é bem útil para quando queremos executar um processamento personalizado antes de salvar o objeto ou até mesmo utilizar outros métodos do modelo. O código ficará conforme abaixo:

```python
from django.shortcuts import render
from visitantes.forms import VisitanteForm

def registrar_visitante(request):

    form = VisitanteForm()
    
    if request.method == "POST":
        form = VisitanteForm(request.POST)
        
        if form.is_valid():
            visitante = form.save(commit=False)

            visitante.registrado_por = request.user.porteiro
            
            visitante.save()    
        
    contexto = {
        "form": form,
    }

    return render(request, "registrar_visitante.html", contexto)
```

Agora, ao invés de salvarmos o formulário diretamente, estamos guardando o resultado do método `save` com o argumento `commit=False`, definindo um valor para o atribudo `registrado_por` diretamente e salvando o objeto através da variável `visitante`. Somente nesse momento que as alterações são registradas no banco de dados.

{% hint style="warning" %}
Lembra que falamos que a variável `request` guarda algumas informações da requisição, como usuário logado e método utilizado? Pois bem, conseguimos pegar informações do usuário logado acessando a propriedade `user` da variável `request` \(`request.user`\). No nosso caso, ainda estamos acessando uma outra propriedade do usuário, a propriedade `porteiro` \(`request.user.porteiro`\).

Isso acontece devido à funcionalidade de acesso entre os modelos que o Django disponibiliza. Assim como podemos acessar `porteiro.usuario`, definido diretamente no modelo, podemos fazer o mesmo para o inverso da relação.
{% endhint %}

Feito isso, vamos apenas importar mais um dos `shortcuts` do Django, além do `render`, que é o `redirect`. O que ele faz é exatamente redirecionar a view para uma URL que quisermos. Vamos utilizá-lo para evitar que os mesmos dados sejam enviados mais de uma vez ao nosso servidor. Sempre que um formulário for enviado e as informações forem salvas no banco de dados, vamos redirecionar a página. Para isso, basta importar o `redirect` ao lado do `render` e utilizá-lo passando a URL para onde queremos redirecionar o usuário. O código ficará assim:

```python
from django.shortcuts import render, redirect
from visitantes.forms import VisitanteForm

def registrar_visitante(request):

    form = VisitanteForm()
    
    if request.method == "POST":
        form = VisitanteForm(request.POST)
        
        if form.is_valid():
            visitante = form.save(commit=False)

            visitante.registrado_por = request.user.porteiro
            visitante.save()
            
            return redirect("index")
        
    contexto = {
        "form": form,
    }

    return render(request, "registrar_visitante.html", contexto)
```

Agora vamos voltar à página [http://127.0.0.1:8000/registrar-vistante/](http://127.0.0.1:8000/registrar-vistante/) e registrar um visitante. O visitante deverá ser registrado e a requisição redirecionada para a página inicial da dashboard.

{% hint style="warning" %}
O visitante registrado deverá estar listado na tabela de visitantes recentes
{% endhint %}

## Exibindo mensagem para o usuário ao cadastrar novo visitante

Agora que o formulário está sendo exibido e funcionando corretamente, inclusive salvando os visitantes em nosso banco de dados, vamos melhorar um pouco a usabilidade da nossa dashboard. Sempre que o sistema finaliza uma ação silicitada pelo usuário, é interessante que seja dado um feedback visual para facilitar o entendimento. Desta forma, o que faremos agora é trabalhar na view para que, quando o visitante for registrado, uma mensagem seja exibida dizendo algo como "hey, cara, o visitante foi registrado com sucesso!".

### Conhecendo o Django messages

Pensando nisso, o Django já nos disponiza o módulo `messages`. Toda a configuração necessária para o funcionamento destas funcionalidades já vêm por padrão quando criamos um novo projeto Django, então o que precisamos fazer é apenas inserir o código `from django.contrib import messages` para importar as funcionalidades. Vamos colocá-lo na primeira linha e o início do arquivo `views.py` do aplicativo **visitantes** ficará assim:

```python
from django.contrib import messages
from django.shortcuts import render, redirect
from visitantes.forms import VisitanteForm

# código abaixo omitido
```

Com o módulo importado em nossa view, podemos utilizá-lo traquilamente. Vamos adicionar uma mensagem de sucesso logo após a linha que salva a instância do visitante \(`visitante.save()`\) utilizando o método sucess do módulo messages e passando a ele a request e um texto para ser exibido. O arquivo `views.py` ficará assim:

```python
from django.contrib import messages
from django.shortcuts import render, redirect
from visitantes.forms import VisitanteForm

def registrar_visitante(request):

    form = VisitanteForm()
    
    if request.method == "POST":
        form = VisitanteForm(request.POST)
        
        if form.is_valid():
            visitante = form.save(commit=False)

            visitante.registrado_por = request.user.porteiro

            visitante.save()
            
            messages.success(,
                request,
                "Visitante registrado com sucesso"
            )
            
            return redirect("index")
        
    contexto = {
        "nome_pagina": "Registrar visitante",
        "form": form,
    }

    return render(request, "registrar_visitante.html", contexto)
```

### Alterando o template para exigir as mensagens

Nossa view para registro de visitantes está completa: estamos exibindo o formulário corretamente, verificando quando ocorre uma requisição do tipo POST, validando as informações enviadas, definindo automaticamente o porteiro que registrou o visitante, exibindo uma mensagem e ainda redirecionamos a requisição quando finalizamos todo o processo com sucesso. Ufa! É tanta coisa que ficou até difícil de listar.

Com tudo isso feito, temos agora que disponibizar um lugar em nosso template para que a mensagem seja exibida, como um alerta mesmo. Como estamos direcionando nosso usuário para a página inicial da dashboard, faz que sentido que a gente coloque a mensagem no template `index.html`, pelo menos por hora.

Vamos abrir o template `index.html` e, logo acima do primeiro elemento `<div class="row">`do arquivo, vamos inserir o seguinte trecho de código:

```markup
{% if messages %}
    {% for message in messages %}
        <div class="alert alert-dismissible alert-{{ message.tags }}" role="alert">
            {{ message }}
            <button type="button" class="close" data-dismiss="alert" aria-label="Close">
                <span aria-hidden="true">&times;</span>
            </button>
        </div>
    {% endfor %}
{% endif %}
```

O módulo de mensagens do Django também nos disponibiliza uma variável chamada `messages`. Com ela, conseguimos verificar se existem mensagens e, por meio de um loop, verificar as informações de cada mensagem. É o que estamos fazendo, primeiro verificamos se existem mensagem \(`{% if messages %}`\), caso positivo, nós executamos um loop e acessamos a mensagem utilizando a variável criada no loop \(`{{ message }}`\).

{% hint style="info" %}
Aqui temos uma novidade, a utilização da tag `{% if %}`. Uma estrutura condicional que pode ser utilizada em templates. O que estiver dentro dela só será exibido caso o resultado da expressão seja verdadeiro. Ou seja, quando existem mensagens e a variável `messages` está definida, exibimos o trecho HTML
{% endhint %}

Agora você pode cadastrar mais um visitante e ver a mensagem de sucesso sendo exibida!

## Tratando possíveis erros em nosso formulário

Nossa mensagem de sucesso já está sendo exibida corrertamente, mas o que acontece se ocorrer algum erro e os dados enviados não forem aceitos? Não podemos deixar que a aplicação pare. Sendo assim, é interessante que a gente também insira um alerta de erro em nosso template.

Como nosso formulário de registro de visitante está no arquivo `registrar_visitante.html`, trabalharemos nele. Logo acima do elemento `<form method="post">`, vamos inserir o seguinte trecho de código:

```markup
{% if form.errors %}
    {% for field in form %}
        {% if field.errors %}
            {% for error in field.errors %}
                <div class="alert alert-dismissible alert-warning" role="alert">
                    {{ error | escape }}
                    <button type="button" class="close" data-dismiss="alert" aria-label="Close">
                        <span aria-hidden="true">&times;</span>
                    </button>
                </div>
            {% endfor %}
        {% endif %}
    {% endfor %}
{% endif %}
```

A estrutura HTML é bem parecida com a utilizada para a mensagem de sucesso, mas com algumas pequenas diferenças. Mais uma vez, utilizaremos as tags `{% if %}` e `{% for %}`. Primeiro vamos verificar se existem erros no formulário \(`{% if form.errors %}`\) e, caso verdadeiro, realizar um loop nos seus campos \(`{% for field in form %}`\). Desta vez, verificamos se existem erros em cada campo \(`{% if field.errors %}`\) e executamos um loop nesses erros \(`{% for error in field.errors %}`\), caso existam.

## Deixando nossas mensagens de erro mais claras

Um último detalhe para melhorarmos ainda mais a experiência do usuário ao utilizar nossa dashboard é certamente melhorar os textos das mensagens que são exibidas para o usuário em caso de erro. Muito mais que informar que ocorreu um erro, essas mensagens devem direcionar o usuário para o correto preenchimento das informações.

Para fazer isso, podemos atuar diretamente no arquivo `forms.py` do aplicativo **visitantes**. O que faremos é adicionar o atributo `error_messages` à classe `VisitanteForm`, logo abaixo de `fields`.

O `error_messages` é um dicionário que deve conter chaves com os nomes dos campos do modelo. Desta forma, cada valor do dicionário `error_messages` representa um campo do formulário e é também um dicionário, mas que, desta vez, recebe como chave os tipos de erros nos formulários do Django seguido da mensagem a ser exibida para cada erro.

{% hint style="info" %}
Por hora utilizaremos os tipos `required`, que funciona para quando o campo não é preenchido e `invalid`, para quando o formato da informação enviada é inválido
{% endhint %}

Nosso formulário, a classe `VisitanteForm`, ficará assim:

```python
class VisitanteForm(forms.ModelForm):
    class Meta:
        model = Visitante
        fields = [
            "nome_completo", "cpf", "data_nascimento",
            "numero_casa", "placa_veiculo",
        ]
        error_messages = {
            "nome_completo": {
                "required": "O nome completo do visitante é obrigatório para cadastro",
            },
            "cpf": {
                "required": "O CPF do visitante é obrigatório para cadastro"
            },
            "data_nascimento": {
                "required": "A data de nascimento do visitante é obrigatória para cadastro",
                "invalid": "Por favor, informe um formato válido para a data de nascimento (DD/MM/AAAA)"
            },
            "numero_casa": {
                "required": "Por favor, informe o número da casa a ser visitada"
            }
        },
```

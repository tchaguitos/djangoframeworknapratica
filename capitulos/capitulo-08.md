# Capítulo 08

## Preparando view para receber requisição do tipo POST

Agora que cuidamos da usabilidade do nosso formulário, podemos seguir com as outras partes da nossa view. Até o momento, apenas criamos a variável que representa o formulário e a passamos no contexto, o que faremos agora é preparar nossa view para receber os dados que serão enviados na requisição.

### Conhecendo o objeto request

Você já deve ter notado que sempre que criamos uma view, precisamos que ela receba a variável `request` como argumento. Isso porque, quando falamos do protocolo que sustenta a web, o HTTP, requisições são o que movimentam toda a estrutura. Quando acessamos uma página web estamos enviando uma requisição do tipo `GET`. Isso tudo acontece em questão de segundos e por baixo dos panos, nos fios que conectam a web.

A variável `request` é uma representação da requisição que é enviada à view acessada e contém diversas informações como usuário logado, método HTTP utilizado, navegador e sistema operacional, dentre outras. No momento, nos interessa o método da requisição e o corpo que é enviado. 

Para prepararmos a view para receber as informações da requisição, vamos adicionar um `if` abaixo da linha `form = VisitanteForm()` para verificar se o método da requisição é do tipo `POST`. Podemos fazer isso dessa forma:

```python
from django.shortcuts import render
from visitantes.forms import VisitanteForm

def registrar_visitante(request):

    form = VisitanteForm()
    
    if request.method == "POST":
        print("o método é post")
    
    context = {
        "nome_pagina": "Registrar visitante",
        "form": form,
    }

    return render(request, "registrar_visitante.html", context)
```

{% hint style="success" %}
O método `POST` é utilizado sempre que precisamos enviar informações para o servidor. No nosso caso, por exemplo, queremos enviar as informações de um novo visitante a ser registrado e fazemos isso através do formulário HTML
{% endhint %}

Agora que verificamos se o método da requisição enviada é do tipo `POST`, vamos reutilizar a variável `form`, agora atribuindo a ela outra instância do formulário `VisitanteForm()`, mas agora passando o corpo da requisição à classe, e utilizar o método `is_valid()` do formulário para validar as informações. O código ficará o seguinte:

```python
from django.shortcuts import render
from visitantes.forms import VisitanteForm

def registrar_visitante(request):

    form = VisitanteForm()
    
    if request.method == "POST":
        form = VisitanteForm(request.POST)
        
        if form.is_valid():
            form.save()
    
    context = {
        "nome_pagina": "Registrar visitante",
        "form": form,
    }

    return render(request, "registrar_visitante.html", context)
```

Para passar o corpo da requisição para o formulário, basta utilizarmos a propriedade POST do objeto `request` e passá-lo como argumento ao criarmos a nova instância do formulário, como feito na linha 9 \(`form = VisitanteForm(request.POST)`\). Feito isso, validamos as informações com o método `is_valid()` e salvamos o formulário utilizando o método `save()`.

## Conhecendo um pouco mais dos formulários

Ao acessarmos a página, podemos notar que todos os campos do modelo estão sendo exibidos no formulário, e não é isso que queremos, pois algumas das informações devem ser preenchidas mediante autorização de moradores e dependem outros eventos.

Para especificar os campos que devem ser exibidos e utilizados no formulário, vamos voltar ao arquivo `forms.py` e alterar o atributo `fields` da classe `Meta` do nosso formulário. Abra o arquivo e substitua a string `"__all__"` por uma lista com os nomes dos campos que vamos exibir. O atributo `fields` ficará assim:

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

Quando criamos a classe modelo `Visitante`, falamos sobre o atributo `registrado_por` ser do tipo `ForeignKey`, um tipo de campo que cria um relacionamento entre as classes `Visitante` e `Porteiro`. Olhando a classe `VisitanteForm`, podemos notar que o atributo não é colocado nos campos do formulário \(`fields`\), mesmo este sendo uma informação obrigatória em nosso modelo. Se tentarmos adicionar um visitante por meio do formulário, o Django apresentará um erro nos informando que o atributo`registrado_por` do modelo não pode ser nulo.

Para resolver o problema, o que vamos fazer é possibilitar que o campo seja preenchido de maneira automática. Isto é, o campo receberá o valor referente ao porteiro que está logado na dashboard no momento do cadastro.

Para fazer isso, antes de salvar o formulário vamos definir diretamente um valor para o atributo `registrado_por` na função de view. Vamos abrir o arquivo `views.py` e criar uma variável para receber o retorno do método `save` do formulário. Esse método aceita também um argumento opcional de nome `commit`, que quando definido como `False`, retorna uma instância do modelo utilizado no formulário que ainda não foi gravada no banco de dados. Isso é bem útil para quando queremos executar um processamento personalizado antes de salvar o objeto ou até mesmo utilizar outros métodos do modelo. O código vai ficar assim:

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
        
    context = {
        "nome_pagina": "Registrar visitante",
        "form": form,
    }

    return render(request, "registrar_visitante.html", context)
```

Agora, ao invés de salvarmos o formulário diretamente, estamos guardando o resultado do método `save` com o argumento `commit=False`, definindo um valor para o atributo `registrado_por` diretamente e salvando o objeto através da variável `visitante`. Somente no momento em que chamamos o método `visitante.save()` que as alterações são registradas no banco de dados.

{% hint style="warning" %}
Lembra que falamos que a variável `request` guarda algumas informações da requisição, como usuário logado e método utilizado? Pois bem, conseguimos pegar informações do usuário logado acessando a propriedade `user` da variável `request` \(`request.user`\). No nosso caso, ainda estamos acessando uma outra propriedade do usuário, a propriedade `porteiro` \(`request.user.porteiro`\).

Isso acontece devido à funcionalidade de acesso entre os modelos que o Django disponibiliza. Assim como podemos acessar `porteiro.usuario`, definido diretamente como atributo do modelo, podemos fazer o mesmo para o inverso da relação.
{% endhint %}

Feito isso, vamos apenas importar mais um dos `shortcuts` do Django, além do `render`, que é o `redirect`. O que ele faz é exatamente redirecionar a view para uma URL que quisermos. Vamos utilizá-lo para evitar que os mesmos dados sejam enviados mais de uma vez ao nosso servidor. Sempre que um formulário for enviado e as informações forem salvas no banco de dados, vamos redirecionar a página. Para isso, basta importar o `redirect` ao lado do `render` e utilizá-lo passando o nome da URL para onde queremos mandar o usuário. O código ficará assim:

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
        
    context = {
        "nome_pagina": "Registrar visitante",
        "form": form,
    }

    return render(request, "registrar_visitante.html", context)
```

Agora vamos voltar à página [http://127.0.0.1:8000/registrar-visitante/](http://127.0.0.1:8000/registrar-visitante) e registrar um visitante. O visitante deverá ser registrado e a requisição redirecionada para a página inicial da dashboard.

{% hint style="warning" %}
O visitante registrado deverá estar listado na tabela de visitantes recentes
{% endhint %}

## Exibindo uma mensagem para o usuário ao cadastrar novo visitante

Agora que o formulário está sendo exibido e funcionando corretamente, inclusive salvando os visitantes em nosso banco de dados, vamos melhorar um pouco a usabilidade da nossa dashboard. Sempre que o sistema finaliza uma ação solicitada pelo usuário, é interessante que seja dado um feedback visual para facilitar o entendimento a respeito do que aconteceu. Desta forma, o que faremos agora é trabalhar na view para que, quando o visitante for registrado, uma mensagem seja exibida dizendo algo como "hey, cara, o visitante foi registrado com sucesso!".

### Conhecendo o Django messages

O Django já nos disponibiliza o módulo `messages` para resolver isso. Toda a configuração necessária para o funcionamento de suas funcionalidades já vêm por padrão quando criamos um novo projeto Django, então o que precisamos fazer é apenas inserir o código `from django.contrib import messages` para importar as funcionalidades e utilizá-las em nossas views. Vamos colocá-lo na primeira linha e o início do arquivo `views.py` do aplicativo **visitantes** ficará assim:

```python
from django.contrib import messages
from django.shortcuts import render, redirect
from visitantes.forms import VisitanteForm

# código abaixo omitido
```

Com o módulo importado em nossa view, podemos utilizá-lo tranquilamente. Vamos adicionar uma mensagem de sucesso logo após a linha que salva a instância do visitante \(`visitante.save()`\) utilizando o método `success` do módulo `messages` e passando a ele a `request` e um texto para ser exibido. O arquivo `views.py` ficará assim:

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
            
            messages.success(
                request,
                "Visitante registrado com sucesso"
            )
            
            return redirect("index")
        
    context = {
        "nome_pagina": "Registrar visitante",
        "form": form,
    }

    return render(request, "registrar_visitante.html", context)
```

### Alterando o template para exibir as mensagens

Nossa view para registro de visitantes está completa: estamos exibindo o formulário corretamente, verificando quando ocorre uma requisição do tipo POST, validando as informações enviadas, definindo automaticamente o porteiro que registrou o visitante, exibindo uma mensagem e ainda redirecionamos a requisição quando finalizamos todo o processo com sucesso. Ufa! É tanta coisa que ficou até difícil de listar.

Com tudo isso feito, temos agora que disponibilizar um lugar em nosso template para que a mensagem seja exibida, como um alerta mesmo. Como estamos direcionando nosso usuário para a página inicial da dashboard, faz sentido que a gente coloque a mensagem no template `index.html`, pelo menos por hora.

Vamos abrir o template `index.html` e, logo acima do primeiro elemento `<div class="row">`do arquivo, vamos inserir o seguinte trecho de código:

```markup
{% if messages %}
    {% for message in messages %}
        <div class="alert alert-success" role="alert">
            {{ message }}
        </div>
    {% endfor %}
{% endif %}
```

O módulo de mensagens do Django também nos disponibiliza uma variável chamada `messages` que pode ser acessada nos templates. Com ela, conseguimos verificar se existem mensagens e, por meio de um loop, verificar as informações de cada mensagem. É o que estamos fazendo, primeiro verificamos se existem mensagem \(`{% if messages %}`\), caso positivo, nós executamos um loop e acessamos a mensagem utilizando a variável criada no loop \(`{{ message }}`\).

Agora você pode cadastrar mais um visitante e ver a mensagem de sucesso sendo exibida!

## Tratando possíveis erros em nosso formulário

Nossa mensagem de sucesso já está sendo exibida corretamente, mas o que acontece se ocorrer algum erro e os dados enviados não forem aceitos? Não podemos deixar que a aplicação pare e precisamos indicar para o usuário que os dados que ele inseriu estão incorretos.

Como nosso formulário de registro de visitante está no arquivo `registrar_visitante.html`, trabalharemos nele. Logo acima do elemento `<form method="post">`, vamos inserir o seguinte trecho de código:

```markup
{% if form.errors %}
    {% for field in form %}
        {% if field.errors %}
            {% for error in field.errors %}
                <div class="alert alert-warning" role="alert">
                    {{ error }}
                </div>
            {% endfor %}
        {% endif %}
    {% endfor %}
{% endif %}
```

A estrutura HTML é bem parecida com a utilizada para a mensagem de sucesso, mas com algumas pequenas diferenças. Mais uma vez, utilizaremos as tags `{% if %}` e `{% for %}`. Primeiro vamos verificar se existem erros no formulário \(`{% if form.errors %}`\) e, caso verdadeiro, realizar um loop nos seus campos \(`{% for field in form %}`\). Desta vez, verificamos também se existem erros em cada campo \(`{% if field.errors %}`\) e executamos um loop nesses erros \(`{% for error in field.errors %}`\), caso existam.

## Deixando nossas mensagens de erro mais claras

Um último detalhe para melhorarmos ainda mais a experiência do usuário ao utilizar nossa dashboard é certamente melhorar os textos das mensagens que são exibidas para o usuário em caso de erro. Muito mais que informar que ocorreu um erro, essas mensagens devem direcionar o usuário para o correto preenchimento das informações.

Para fazer isso, vamos atuar diretamente no arquivo `forms.py` do aplicativo **visitantes**. O que faremos é adicionar o atributo `error_messages` à classe `Meta` da classe `VisitanteForm`, logo abaixo de `fields`.

O `error_messages` é um dicionário que deve conter chaves com os nomes dos campos do modelo. Desta forma, cada valor do dicionário `error_messages` representa um campo do formulário e é também um dicionário, mas que, desta vez, recebe como chave os tipos de erros que podem acontecer nos formulários do Django seguido da mensagem a ser exibida para cada erro. Por hora utilizaremos os tipos `required`, que funciona para quando o campo não é preenchido e `invalid`, para quando o formato da informação enviada é inválido.

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
                "required": "O nome completo do visitante é obrigatório para o registro",
            },
            "cpf": {
                "required": "O CPF do visitante é obrigatório para o registro"
            },
            "data_nascimento": {
                "required": "A data de nascimento do visitante é obrigatória para o registro",
                "invalid": "Por favor, informe um formato válido para a data de nascimento (DD/MM/AAAA)"
            },
            "numero_casa": {
                "required": "Por favor, informe o número da casa a ser visitada"
            }
        }
```


# Capítulo 06

## Criando o aplicativo para gerenciar visitantes

No último capítulo configuramos o Django para trabalhar com templates HTML e arquivos estáticos e criamos as pastas **templates** e **static** na raiz do projeto. Além disso, ainda aprendemos como passar informações de uma função _view_ para o template através da variável **context**.

Como já definimos os usuários do sistema, os porteiros e fizemos as configurações dos templates que serão a base para construção da dashboard para controle de visitantes, podemos partir agora para a definição da classe modelo que irá representar os visitantes em nosso sistema.

Antes de mais nada, como já sabemos, devemos isolar as responsabilidades e, por isso, vamos criar um aplicativo para administrar toda a parte referente aos nossos visitantes. Vamos criar um novo aplicativo com nome de **visitantes** utilizando o `manage.py`:

```bash
(env)$ python manage.py startapp visitantes
```

Após criarmos o aplicativo utilizando o `manage.py`, vamos registrá-lo no arquivo de configurações, o `settings.py`, logo abaixo do aplicativo **porteiros**:

```python
INSTALLED_APPS += [
    "usuarios",
    "porteiros",
    "visitantes",
]
```

Feito isso, vamos começar os trabalhos no arquivos `models.py` para definirmos as informações necessárias para o modelo de visitantes.

## Escrevendo as models do nosso aplicativo de visitantes

Conforme falamos, a camada _model_ ****\(ou camada de modelo\) é nossa fonte segura de dados e onde definimos o formato das informações que serão disponibilizadas para outras camadas da aplicação.

Precisamos guardar uma série de informações a respeito de quem deseja adentrar ao condomínio para realizar visitas a moradores, além da autorização de um morador que esteja na casa no momento da visita. O procedimento faz parte de normas do condomínio para fins de fiscalização, controle e segurança dos moradores. Segundo normas do condomínio, devemos guardar as seguintes informações referentes à visita:

1. Nome completo do visitante
2. CPF do visitante
3. Data de nascimento do visitante
4. Número da casa a ser visitada
5. Placa do veículo utilizado na visita, se houver
6. Horário de chegada na portaria
7. Horário de saída do condomínio
8. Horário de autorização de entrada
9. Nome do morador responsável por autorizar a entrada do visitante
10. Porteiro responsável por registrar visitante

Inicialmente, vamos nos concentrar nas informações de 1 a 5 para que possamos avaliar e escrever por partes o modelo de visitantes. Vamos escrever primeiro os atributos nome completo, CPF, data de nascimento, número da casa e placa do veículo, pois a gente já conhece a maioria desses tipos de dados. Nossa classe `Visitante` ficará assim:

```python
from django.db import models

class Visitante(models.Model):
    nome_completo = models.CharField(
        verbose_name="Nome completo", max_length=194
    )

    cpf = models.CharField(
        verbose_name="CPF",
        max_length=11,
    )

    data_nascimento = models.DateField(
        verbose_name="Data de nascimento",
        auto_now=False,
        auto_now_add=False,
    )

    numero_casa = models.PositiveSmallIntegerField(
        verbose_name="Número da casa a ser visitada"
    )

    placa_veiculo = models.CharField(
        verbose_name="Placa do veículo",
        max_length=7,
        blank=True,
        null=True,
    )
```

### Conhecendo o campo DateTimeField

Antes de prosseguirmos, vamos conhecer o campo `DateTimeField`, um cara bem parecido com o `DateField` que já conhecemos com a diferença que, além da data, salva também o horário exato do registro. Assim como o `DateField`, o `DateTimeField` aceita `auto_now` e `auto_now_add` como argumentos, além das opções `blank` e `null`. Vamos utilizar o `DateTimeField` para definirmos os atributos que vão representar o horário de chegada e o horário de saída do visitante.

Primeiro vamos definir o atributo `horario_chegada`, que é quem representa o horário de chegada do visitante à portaria do condomínio. Como o atributo representa o horário de chegada do visitante à portaria, faz sentido que o mesmo seja preenchido no exato momento que registrarmos o visitante em nosso sistema. Para isso, utilizaremos a opção `auto_now_add` com o valor `True`, assim garantimos que o atributo receberá o valor da hora atual assim que o registro for adicionado ao banco de dados.

```python
from django.db import models

class Visitante(models.Model):
    # código acima omitido...

    placa_veiculo = models.CharField(
        verbose_name="Placa do veículo",
        max_length=7,
        blank=True,
        null=True,
    )
    
    horario_chegada = models.DateTimeField(
        verbose_name="Horário de chegada na portaria",
        auto_now_add=True
    )
```

Para o caso do horário de saída, utilizaremos uma configuração diferente para o atributo. Como precisamos setar o valor somente após a saída do visitante do condomínio, esse valor precisa ser registrado inicialmente como um valor em branco. Para isso, o Django nos dá a possibilidade de utilização do atributo `auto_now` como `False` e dizer que podemos aceitar valores em branco e nulos. Utilizando o `auto_now` como `False`, o campo receberá um valor em branco no momento da criação do registro no banco de dados.

```python
from django.db import models

class Visitante(models.Model):
    # código acima omitido...

    placa_veiculo = models.CharField(
        verbose_name="Placa do veículo",
        max_length=7,
        blank=True,
        null=True,
    )
    
    horario_chegada = models.DateTimeField(
        verbose_name="Horário de chegada na portaria",
        auto_now_add=True
    )
    
    horario_saida = models.DateTimeField(
        verbose_name="Horário de saída do condomínio",
        auto_now=False,
        blank=True,
        null=True,
    )
```

Conforme visto, além destas informações, precisamos guardar também o nome do morador que autorizou a entrada do visitante e o horário da autorização. Sendo assim, teremos mais dois atributos:

* Horário de autorização de entrada
* Nome do morador responsável por autorizar a entrada do visitante

Utilizaremos os campos já conhecidos `DateTimeField` e `CharField` para definir os atributos:

```python
from django.db import models

class Visitante(models.Model):
    # código acima omitido...
    
    horario_saida = models.DateTimeField(
        verbose_name="Horário de saída do condomínio",
        auto_now=False,
        blank=True,
        null=True,
    )
    
    horario_autorizacao = models.DateTimeField(
        verbose_name="Horário de autorização de entrada",
        auto_now=False,
        blank=True,
        null=True,
    )
    
    morador_responsavel = models.CharField(
        verbose_name="Nome do morador responsável por autorizar a entrada do visitante",
        max_length=194,
        blank=True,
    )
```

Nada de novo por enquanto: utilizamos um campo do tipo `CharField` para armazenar o nome do morador responsável por autorizar a entrada e utilizamos um `DateTimeField` para armazenar o horário em que a autorização ocorreu. Conforme vimos, se não queremos que o campo `DateTimeField` seja preenchido na hora da criação ou atualização do registro, setamos o argumento `auto_now` como `False`. Isso garante também que o campo possa ser preenchido com um texto vazio.

### Conhecendo o campo ForeignKey

Seguindo a lista de requisitos, o próximo atributo que devemos adicionar ao nosso modelo é a informação referente ao **porteiro responsável por registrar a entrada do visitante**.

Como criamos um modelo que representa nossos porteiros dentro do sistema, podemos associar esse modelo ao modelo de **visitante** por meio do campo `ForeignKey`. Esse campo cria um atributo que vincula um registro de modelo a outro registro de modelo. Neste caso, queremos vincular o modelo `Porteiro` ao modelo `Visitante` para representar o porteiro responsável pelo registro.

O campo `ForeignKey` é importante pois assim não precisamos replicar as informações do porteiro no registro de visitante, apenas referenciamos essas informações inserindo o `id` referente ao registro do **porteiro**. Como os modelos são representações das tabelas do nosso banco de dados, estamos dizendo algo como: _"hey, Django, procure essas informações na tabela de porteiros usando esse id!"_ e o Django faz todo o trabalho de trazer essas informações por nós.

{% hint style="info" %}
É importante lembrar que tudo isso é possível devido à ORM do Django, que é quem abstrai todas as funcionalidades do banco de dados e manipula todas as interações necessárias
{% endhint %}

Abaixo do atributo `morador_responsavel`, vamos escrever o atributo  `registrado_por` sendo do tipo `ForeignKey`, que representará a informação do **porteiro** responsável por registrar a entrada do visitante:

```python
from django.db import models

class Visitante(models.Model):
    # código acima omitido...
    
    morador_responsavel = models.CharField(
        verbose_name="Nome do morador responsável por autorizar a entrada do visitante",
        max_length=194,
        blank=True,
    )
    
    registrado_por = models.ForeignKey(
        "porteiros.Porteiro",
        verbose_name="Porteiro responsável pelo registro",
        on_delete=models.PROTECT
    )
```

Os argumentos que o campo `ForeignKey` recebe são bem parecidos com os do `OneToOneField` que já conhecemos. Primeiro informamos a classe modelo que deverá ser relacionada: a classe `Porteiro` do aplicativo **porteiros**. Depois utilizamos o `verbose_name` para dar um nome descritivo para o campo e informamos o que deve ser feito com o registro do visitante caso o registro do **porteiro** da relação seja excluído. Nesse caso, utilizaremos para o `on_delete` a ação `models.PROTECT`, pois assim protegemos também o modelo de visitantes. Sempre que houver tentativa de exclusão de um registro de porteiro que esteja vinculado a um visitante, o Django mostrará um erro. O atributo está protegido contra exclusão.

Nosso próximo passo agora é apenas escrever a classe `Meta` e o método `__str__` da classe Visitante. Nosso modelo ficará assim:

```python
from django.db import models

class Visitante(models.Model):
    nome_completo = models.CharField(
        verbose_name="Nome completo", max_length=194
    )

    cpf = models.CharField(
        verbose_name="CPF",
        max_length=11,
    )

    data_nascimento = models.DateField(
        verbose_name="Data de nascimento",
        auto_now=False
    )

    numero_casa = models.PositiveSmallIntegerField(
        verbose_name="Número da casa a ser visitada",
    )

    placa_veiculo = models.CharField(
        verbose_name="Placa do veículo",
        max_length=7,
        blank=True,
        null=True,
    )
    
    horario_chegada = models.DateTimeField(
        verbose_name="Horário de chegada na portaria",
        auto_now_add=True
    )
    
    horario_saida = models.DateTimeField(
        verbose_name="Horário de saída do condomínio",
        auto_now=False,
        blank=True,
        null=True,
    )
    
    horario_autorizacao = models.DateTimeField(
        verbose_name="Horário de autorização de entrada",
        auto_now=False,
        blank=True,
        null=True,
    )
    
    morador_responsavel = models.CharField(
        verbose_name="Nome do morador responsável por autorizar a entrada do visitante",
        max_length=194,
        blank=True,
    )

    registrado_por = models.ForeignKey(
        "porteiros.Porteiro",
        verbose_name="Porteiro responsável pelo registro",
        on_delete=models.PROTECT
    )
    
    class Meta:
        verbose_name = "Visitante"
        verbose_name_plural = "Visitantes"
        db_table = "visitante"

    def __str__(self):
        return self.nome_completo
```

## Registrando nossa aplicação no Admin do Django

O próximo passo a ser executado, como já vimos, é tornar o nosso modelo visível para o Admin do Django. Então vamos lá!

Vamos abrir o arquivo `admin.py` do nosso aplicativo visitantes, importar a classe `Visitante` e passá-la como argumento da função `admin.site.register()`.

```python
from django.contrib import admin
from visitantes.models import Visitante

admin.site.register(Visitante)
```

## Aplicando as alterações em nosso banco de dados

Feito isso, mais uma vez vamos criar as migrações do modelo criado utilizando o comando `makemigrations`.

```bash
(env)$ python manage.py makemigrations visitantes
```

Se tudo ocorrer bem, vamos receber as seguintes informações em nosso terminal:

```text
Migrations for 'visitantes':
  porteiros/migrations/0001_initial.py
    - Create model Visitante
```

Com todas as informações necessárias para executar as alterações no banco de dados armazenadas em forma de migração, vamos aplicar as alterações em nosso banco de dados com o comando `migrate`.

```bash
(env)$ python manage.py migrate
```

Devemos receber as seguintes informações em nosso terminal:

```text
Operations to perform:
  Apply all migrations: admin, auth, contenttypes, porteiros, sessions,
  usuarios, visitantes

Running migrations:
  Applying visitantes.0001_initial.py... OK
```

## Adicionando visitante utilizando o Django Admin

Da mesma forma que cadastramos um porteiro utilizando o Admin do Django, vamos adicionar também um visitante. Vamos novamente acessar [http://127.0.0.1:8000/admin](http://127.0.0.1:8000/admin) e agora veremos o aplicativo **visitantes** disponível para nós.

![Captura tirada do Admin do Django focando nos links para a exibi&#xE7;&#xE3;o de Grupos, Usu&#xE1;rios, Porteiros e agora Visitantes tamb&#xE9;m](../.gitbook/assets/screenshot_2020-02-20_20-48-46.png)

Dessa vez, vamos clicar diretamente no botão "adicionar" para que a gente vá direto para o formulário de cadastro de visitantes. O formulário a ser exibido deverá se parecer com isto:

![Formul&#xE1;rio para registro de visitante. O formul&#xE1;rio apresenta os campos da classe modelo](../.gitbook/assets/screenshot_2020-02-20_20-56-30.png)

Por enquanto vamos preencher os campos obrigatórios **nome completo**, **CPF**, **data de nascimento** e **número da casa** a ser visitada e definir o **porteiro** responsável por registrar o visitante. Fique livre para preencher as informações à sua maneira. Após preencher os campos citados, clique em salvar e visualize a lista de visitantes agora com o novo visitante registrado.

Essa foi a primeira e última vez utilizamos o Django Admin para registar um visitante, pois a partir de agora trabalharemos diretamente nos templates HTML da dashboard que vamos disponibilizar para os porteiros do condomínio.

## Listando visitantes na página inicial da dashboard

Como já definimos nosso modelo e até registramos visitantes através do Admin, agora vamos aprender como buscar esses registros em nosso banco de dados.

Quando precisamos buscar registros em nosso banco de dados, devemos construir uma **Queryset** utilizando o **Manager** da classe modelo em questão. A classe Manager, conforme visto, define como as interações com o banco de dados devem acontecer e, por padrão, é um atributo da classe chamado `objects`. Já uma queryset nada mais é que uma lista de objetos de um determinado tipo existentes em nosso banco de dados. 

Sempre que definimos uma subclasse de `django.db.models.Model`, que é o que todos os nossos modelos são, o Django nos fornece de forma automática uma interface para realizar operações em nosso banco de dados, tais como buscar, atualizar, criar e deletar registros, mas por hora, vamos nos concentrar apenas em buscar os registros de visitantes.

### Buscando registros de visitantes no banco de dados

Quando falamos da camada **view**, vimos que é ela quem deve encapsular toda a lógica necessária para apresentar os dados. Geralmente, as **views** devem buscar as informações no banco de dados, carregar o template e renderizar esse template com as informações buscadas. Uma view no Django tem a função de exatamente conectar a camada de modelo à camada de template.

O primeiro passo para buscarmos os registros é criar uma variável para armazenar os registros que serão retornados, para isto utilizaremos a variável `todos_visitantes`. A variável receberá uma **queryset** que será retornada pelo método `all()` do **Manager** `objects` do modelo **Visitante**.

Antes de tudo, vamos importar o modelo no arquivo `views.py` do aplicativo **usuarios**.

```python
from django.shortcuts import render

from visitantes.models import Visitante

def index(request):
    context = {
        "nome_pagina": "Início da dashboard",
    }
    
    return render(request, "index.html", context)
```

Feito isso, vamos criar a variável `todos_visitantes` acima da variável `context` e definir seu valor como `Visitante.objects.all()`. Desta forma, estamos buscando todos os registros de visitantes existentes em nosso banco de dados.

Não podemos nos esquecer de colocar a variável `todos_visitantes` dentro do nosso dicionário `context` para que possamos acessá-la através do template. A função `index` ficará assim:

```python
from visitantes.models import Visitante

def index(request):

    todos_visitantes = Visitante.objects.all()
    
    context = {
        "nome_pagina": "Início da dashboard",
        "todos_visitantes": todos_visitantes,
    }
    
    return render(request, "index.html", context)
```

### Listando registros de visitantes no template HTML

#### Conhecendo a tag for e acessando atributos do visitante

Para exibir variáveis nos templates vimos que podemos utilizar a sintaxe de chaves \(`{{  }}`\), mas se tentarmos exibir uma queryset desta maneira, não será possível, pois uma queryset é uma lista que contém vários itens. É necessário percorrer os itens dessa lista e acessar item por item. Pra nossa sorte, o Django nos fornecer uma tag para que possamos executar loops em listas.

A tag `{% for %}` é quem vai nos ajudar agora. O que ela faz é justamente andar por todos os itens da lista e disponibilizar uma variável para que possamos acessar as informações da mesma. Parece um pouco confuso? Não se assuste, vamos entender melhor visualizando o código.

No nosso caso, buscamos todos os registros de visitantes que temos em nosso banco de dados e passamos essa lista como variável dentro do contexto da view. O que precisamos fazer agora é passar em cada item existente na lista de visitantes e exibir as informações como nome completo, CPF e data de nascimento.

{% hint style="info" %}
Para acessarmos as informações dos visitantes nos templates, utilizaremos o nome dos atributos definidos da classe modelo.
{% endhint %}

Vamos abrir o arquivo de template `index.html` e buscar pelo elemento HTML `<tbody>`. É dentro dele, acima dos elementos `<tr>` que vamos definir o início da tag `{% for %}`. Para utilizar essa tag, precisamos também evidenciar onde o loop deve ser parado e fazemos isso utilizando a tag `{% endfor %}`.

Logo abaixo do elemento `<tbody>` insira o trecho `{% for visitante in todos_visitantes %}`. Lembra que falamos da variável que a tag `{% for %}` disponibiliza para que possamos acessar as informações? Pois bem, podemos dar nome a ela e, neste caso, utilizaremos o nome `visitante`. O trecho de código ficará assim:

```markup
<tbody>
    {% for visitante in todos_visitantes %}
        <tr>
            <td>Don Corleone</td>
            <td>123.123.123.06</td>
            <td>22 de agosto 15:30</td>
            <td>22 de agosto 15:38</td>
            <td>Darth Vader</td>
            <td>
                <a href="#">
                    Ver detalhes
                </a>
            </td>
        </tr>
    {% endfor %}
</tbody>
```

Olha só que bacana: estamos dizendo para o Django "hey, cara, para cada `visitante` que existir na lista `todos_visitantes`, repita essa estrutura de elementos `<td>`. Com isso já estamos executando o loop na lista `todos_visitantes`, mas ainda não estamos exibindo os valores referentes a cada visitante existente no banco de dados. Para fazer isso, vamos utilizar a sintaxe de chaves \(`{{  }}`\) em conjunto com variável `visitante` que criamos dentro da tag `{% for %}`.

Os atributos do visitante podem ser acessados utilizando a sintaxe `visitante.nome_do_atributo`. Se queremos exibir o nome completo do visitante, vamos utilizar `visitante.nome_completo`. Faremos o mesmo com os atributos **CPF**, **horário de chegada**, **horário de autorização de entrada** e **morador responsável**. Realizando as alterações para exibirmos os atributos, o código ficará assim:

```markup
<tbody>
    {% for visitante in todos_visitantes %}
        <tr>
            <td>{{ visitante.nome_completo }}</td>
            <td>{{ visitante.cpf }}</td>
            <td>{{ visitante.horario_chegada }}</td>
            <td>{{ visitante.horario_autorizacao }}</td>
            <td>{{ visitante.morador_responsavel }}</td>
            <td>
                <a href="#">
                    Ver detalhes
                </a>
            </td>
        </tr>
    {% endfor %}
</tbody>
```

Agora quando atualizarmos a página, vamos visualizar as informações dos visitantes registrados através do Admin. Caso queira testar, fique à vontade para registrar outros visitantes. Quando você acessar novamente [http://127.0.0.1:8000/](http://127.0.0.1:8000/) e atualizar a página, os novos visitantes serão adicionados à tabela de forma automática! Bem bacana, não?


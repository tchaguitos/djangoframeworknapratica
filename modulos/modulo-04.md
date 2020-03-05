# Módulo 04

## Configurando a aplicação para trabalhar com arquivos estáticos e templates HTML

Nos capítulos anteriores, criamos toda a estrutura necessária para criação de usuários do sistema e porteiros que serão os responsáveis por operar a dashboard proposta. Além disso, também preparamos o ambiente de desenvolvimento e aprendemos bastante sobre detalhes técnicos do funcionamento do Django.

Focamos nosso trabalho nos arquivos `models.py` e `admin.py` e também conhecemos o poder existente do Admin que o Django nos disponibiliza. Como nosso objetivo é desenvolver uma dashboard personalizada para exibir as informações dos visitantes do condomínio e implementar funcionalidades específicas, a partir de agora, trabalharemos para desenvolver os templates que irão dispôr as informações necessárias e funcionalidades específicas! 

Sendo assim, posso dizer que a partir de agora as coisas começam a ficar mais interessantes!

Neste próximo módulo, aprenderemos a configurar o Django para trabalhar com arquivos estáticos \(CSS e JS\) e templates HTML.

{% hint style="warning" %}
O Django, por padrão, vem configurado para que já seja possível trabalhar com templates HTML, mas vamos alterar as configurações para que a haja uma maior organização dos arquivos
{% endhint %}

### Criando a pasta templates em nosso projeto

Sendo um framework web, o Django precisa fornecer uma maneira de gerar os templates de forma dinâmica, afim de exibir valores específicos para atender os diversos cenários. Essencialmente, um template é constituído por uma parte estática e uma parte onde serão exibidas as informações desejadas, sendo necessário seguir uma sintaxe específica para exibição de valores e funções fornecidas pelo framework. O Django nos fornece uma _engine_ rica e poderosa capaz de executar funções condicionais, loops, exibir valores e ainda possui diversas funcionalidades que podem ser utilizadas diretamente nos templates HTML através de tags.

{% hint style="info" %}
Uma engine de template nada mais é que uma aplicação que visa facilitar o processo de criação de templates HTML dinâmicos e tornar o processo de envio e exibição de informações nos templates menos burocrático
{% endhint %}

Por padrão, o Django vem configurado para procurar os templates dentro de cada aplicativo. Isto é, em cada aplicativo deverá existir uma pasta **templates** para armazenar os templates referentes ao aplicativo em questão. Todavia, para uma melhor organização, utilizaremos uma pasta externa para armazenar os arquivos de templates do projeto.

Para isso, começaremos alterando o arquivo `settings.py` do nosso projeto. Nesse arquivo é possível encontrar a variável `TEMPLATES`, que é responsável por definir as configurações de template do Django, como _engine_ a ser utilizada, diretórios que armazenam os templates, dentre outras.

A variável `TEMPLATES` é uma lista que recebe um dicionário contendo valores específicos, tais como `BACKEND`, `DIRS`, `APP_DIRS` e `OPTIONS`, cada um com uma função específica. No nosso caso, vamos alterar o valor `DIRS` de uma lista vazia para uma lista contendo a string "templates", que é o nome da pasta que utilizaremos para armazenar nossos templates na raíz do projeto.

Nossa variável `TEMPLATES` ficará da seguinte forma:

```python
TEMPLATES = [
    {
        "BACKEND": "django.template.backends.django.DjangoTemplates",
        "DIRS": ["templates"],
        "APP_DIRS": True,
        "OPTIONS": {
            "context_processors": [
                "django.template.context_processors.debug",
                "django.template.context_processors.request",
                "django.contrib.auth.context_processors.auth",
                "django.contrib.messages.context_processors.messages",
            ],
        },
    },
]
```

Para facilitar as coisas e economizar um pouquinho de tempo, você pode fazer download da pasta **templates** zipada, extrair os arquivos e colocá-la na raíz do seu projeto:

{% file src="../.gitbook/assets/templates \(1\).zip" caption="Iniciar o download" %}

### Criando a pasta static em nosso projeto

Feito isso, vamos agora definir as configurações para os arquivos estáticos do nosso projeto. Assim como para os templates, o Django também nos dá toda a estrutura necessária para trabalharmos com arquivos estáticos.

Por "arquivos estáticos", entenda arquivos do tipo CSS, JS \(javascript\) e imagens que serão utilizadas em nossos templates, tais como logotipo, imagem padrão para avatar de usuários, dentre outras.

Para realizarmos a configuração, vamos novamente alterar o arquivo `settings.py.` Ao final do arquivo, adicione o seguinte trecho de código para definir a URL onde os arquivos estáticos serão armazenados:

```python
STATIC_URL = "/static/"
```

Abaixo do `STATIC_URL`, coloque também o seguinte trecho de código:

```python
STATICFILES_DIRS = [
    os.path.join(BASE_DIR, "static")
]
```

Estamos dizendo ao Django: hey, amigo, procure pelos arquivos estáticos na pasta **static** na raíz do projeto!

Para facilitar e economizar tempo novamente, faça download da pasta zipada clicando no link abaixo \(agora da pasta **static**, claro\) e a coloque na raiz do projeto:

{% file src="../.gitbook/assets/static.zip" caption="Iniciar o download" %}

Faça o download e coloque a pasta **static** na raíz do projeto, juntamente com a pasta **templates**. Feito isso, já podemos utilizar templates HTML e arquivos estáticos em nosso projeto!

## Criando views que renderizam templates

Falamos bastante sobre templates HTML, mas o que é HTML, afinal?

O HTML \(Linguagem de Marcação de HiperTexto\) é o bloco de construção mais básico da pilha de tecnologias que compõem a web, mas é ela que define significado e estrutura de conteúdo. Existem também tecnologias que descrevem aparência/apresentação \(CSS\) e funcionalidade/comportamento \(Javascript\) de uma página web \(inclusive já falamos um pouquinho delas por aqui quando falamos sobre arquivos estáticos\).

Basicamente, um arquivo de template é um arquivo de texto com extensão `.html`. Os navegadores interpretam esses arquivos de texto e cuidam de de exibir exatamente da maneira que você enxerga pelo seu monitor. 

Bacana não? Além disso, o HTML ajuda a dizer para os motores de busca o que é relevante, o que é texto, o que é imagem e tudo mais. Sendo assim, o HTML tem um papel fundamental dentro da web! 

{% hint style="info" %}
E ah, não se assuste com a palavra **HiperTexto** no nome, hipertexto são apenas os links entre as páginas que se conectam na web. 
{% endhint %}

Como o Django já nos dá tudo que é necessário para criarmos aplicações web, ele também nos dá a possibilidade de criarmos views que renderizam templates. Isto é, a partir de agora, ao invés de ser exibido um texto ao acessarmos uma URL através do navegador, como fizemos anteriormente, vamos dizer para o Django que é necessário exibir um template HTML, afim de exibir as informações de forma estruturada e de modo que fiquei fácil a compreensão para nossos usuários.

### Conhecendo a função render

Para que o Django exiba um template ao invés de um texto em tela, precisaremos alterar o retorno da nossa view denominada `index`. Antes de seguir, vamos trabalhar um pouco a memória e lembrar como a nossa view está:

```python
from django.http import HttpResponse

def index(request):
    return HttpResponse("Olá, mundo!")
```

Até então, utilizamos o `HttpResponse` para retornar uma mensagem, mas a partir de agora utilizaremos a função `render` para exibir um template HTML no lugar da mensagem. 

A função `render` é uma função de atralho do Django que nos possibilita combinar um template HTML e um dicionário de contexto. A função deve receber sempre a variável `request` e uma `string` representando o caminho do template a ser utilizado. Esses argumentos são obrigatórios e devem ser passados para a função `render` sempre que a mesma for utilizada, caso contrário, ocorrerá um erro.

Vamos alterar a nova view para que retorne a função `render` ao invés da classe `HttpResponse` passando a variável `request` e o caminho para o template `index.html`:

```python
def index(request):
    return render(request, "index.html")
```

Como já fizemos o download das pastas **static** e **templates** e toda a configuração necessária para funcionamento de ambas, o Django já reconhece a pasta e busca pelo template `index.html` dentro dela.

## Entendendo as adaptações necessárias no template

Como você deve ter percebido, o template não está sendo exibido como deveria. Isso porque os arquivos estáticos não foram carregados. Lembra que fizemos a configuração da variável `STATIC_URL`? Pois bem, precisamos falar dela aqui pois para que os arquivos sejam carregados corretamente, o caminho relativo até eles deve estar correto e é aqui que a `STATIC_URL` entra em cena.

### Conhecendo a tag static

Quando falamos anteriormente sobre a engine de templates do Django, falamos que ela é capaz de executar funções condicionais, loops, exibir valores e possui diversas outras funcionalidades que podem ser executadas diretamente nos templates através de tags. Vamos agora conhecer a primeira tag que vamos utilizar em nosso projeto, a tag **static**.

A tag static é a representação da variável `STATIC_URL` nos templates. O Django fornece essa tag no intuito de facilitar o trabalho com arquivos estáticos. Vamos aprender como utilizar a tag e resolver o problema de exibição do template.

O primeiro passo para utilizarmos a tag static é carregá-la no template. Para isso vamos inserir o seguinte trecho de código no topo do nosso HTML:

```markup
<!DOCTYPE html>

{% load static %}

<html lang="pt-br">
```

{% hint style="success" %}
Tags no Django são escritas dessa maneira: `{% %}`. Guarde isso, pois utilizaremos bastante no decorrer do curso. 
{% endhint %}

Com isso já podemos utilizar a tag no template `index.html`.

### Alterando o caminho dos arquivos estáticos

Como falamos, a tag `{% static %}` é a representação da pasta **static.** A tag nos dá um link para essa pasta para utilização no carregamento dos arquivos estáticos nos templates. Como é o Django que cuida de toda essa parte por nós, também vamos delegar a ele o carregamento dos nossos arquivos JS, CSS e imagens.

#### Alterando importação de arquivos CSS

Vamos alterar primeiro as importações dos arquivos CSS. As linhas que fazem o carregamento dos arquivos CSS no template são:

```markup
<link href="css/sb-admin-2.min.css" rel="stylesheet">
<link href="vendor/fontawesome-free/css/all.min.css" rel="stylesheet" type="text/css">
```

Vamos alterar os textos referentes a `href` para utilizarmos a tag `{% static %}`. As importações ficarão assim:

```markup
<link href="{% static 'css/sb-admin-2.min.css' %}" rel="stylesheet">    
<link href="{% static 'vendor/fontawesome-free/css/all.min.css' %}" rel="stylesheet" type="text/css">
```

#### Alterando importação de arquivos JS

Para alterarmos as importações dos arquivos JS, vamos encontrar as linhas:

```markup
<script src="vendor/jquery/jquery.min.js"></script>
<script src="vendor/bootstrap/js/bootstrap.bundle.min.js"></script>
<script src="js/sb-admin-2.min.js"></script>
```

E alterá-las para que fiquem da seguinte forma:

```markup
<script src="{% static 'vendor/jquery/jquery.min.js' %}"></script>
<script src="{% static 'vendor/bootstrap/js/bootstrap.bundle.min.js' %}"></script>
<script src="{% static 'js/sb-admin-2.min.js' %}"></script>
```

Com isso, ao acessarmos [`http://127.0.0.1:8000/`](http://127.0.0.1:8000/) novamente no navegador, teremos o template exibido de forma estruturada com os arquivos CSS \(exibição\) e JS \(comportamento\) devidamente carregados.

## Exibindo variáveis no template

Quando conhecemos a função `render` falamos que ela é responsável por combinar um template HTML e um dicionário de contexto, mas não fomos procurar o que é um dicionário de contexto. Agora é hora de falarmos dele.

Um dicionário de contexto é a variável do tipo dicionário que pode ser passada como argumento para a função render. Quando passada, é possível acessarmos os valores contidos na variável diretamente no template através de tags específicas.

### Definindo nosso dicionário de contexto

Para fazer isso, vamos no arquivo `views.py` e vamos criar a variável `contexto` acima do retorno da função. A função index ficará da seguinte forma:

```python
def index(request):
    
    contexto = {
        "nome_curso": "Django framework na prática",
    }
    
    return render(request, "index.html", contexto)
```

{% hint style="info" %}
No código acima criamos a variável `contexto` já com o valor `usuario_logado` definido. Para identificarmos o usuário logado, vamos pedir uma ajuda à variável `request` que nos dá a possibilidade de acesso à informações `user` que representa o usuário logado na requisição em questão
{% endhint %}

### Exibindo as informações nos templates

A partir de agora, vamos aprender um pouco mais sobre a linguagem de templates do Django. Ela foi projetada para ser poderosa e fácil de formma que seja confortável trabalhar com a linguagem HTML.

Essencialmente, templates são arquivos de texto, geralmente nos formatos HTML, XML ou CSV. Para o Django, um template contém variáveis que podem ser substituídas por valores quando o template é interpretado e tags que controlam a lógica do template.

Agora que já definimos o nosso dicionário de contexto e passamos ele como argumento para a função `render`, vamos exibir essas informações no template `index.html`.

{% hint style="success" %}
Dicionário é uma estrutura de dados em Python e seus elementos não são ordenados. Dicionários são estruturas poderosas e muito utilizadas pois podemos acessar seus elementos através de chaves. Existem linguagens que este tipo é conhecido como "matrizes associativas" ou apenas "objetos"
{% endhint %}

Para exibirmos os valores contidos no dicionário `contexto` basta utilizarmos a sintaxe para variáveis da linguagem de templates do Django: `{{ chave_do_dicionario }}`. Para nosso caso, vamos exibir o valor da chave `nome_curso` que, dentro do dicionário `contexto`, corresponde ao texto **Django framework na prática**. 

Vamos abrir o arquivo index.html e procurar pela seguinte linha:

```python
<h1 class="h3 mb-4 text-gray-800">Página inicial</h1>
```

Vamos alterar o texto da tag `h1` \(o texto **Página inicial**\) para exibir também o valor da nossa variável `nome_curso` passada no `contexto` do template. A linha deverá ficar assim:

```python
<h1 class="h3 mb-4 text-gray-800">Página inicial - {{ nome_curso }}</h1>
```

Volte para o navegador, atualize a página e veja a mágica acontecer: o valor `{{ nome_curso }}` será substituído pelo texto "Django framework na prática" que definimos no dicionário `contexto`. Se alterarmos o valor no arquivo `views.py` o mesmo acontece no `index.html`.


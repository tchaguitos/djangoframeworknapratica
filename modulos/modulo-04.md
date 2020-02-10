# Módulo 04 - in dev

## Configurando a aplicação para trabalhar com arquivos estáticos e templates HTML

Nos capítulos anteriores, criamos toda a estrutura necessária para criação de usuários do sistema e porteiros que serão os responsáveis por operar a dashboard proposta. Além disso, também preparamos o ambiente de desenvolvimento e aprendemos bastante sobre detalhes técnicos do funcionamento do Django.

Focamos nosso trabalho nos arquivos `models.py` e `admin.py` e também conhecemos o poder existente do Admin que o Django nos disponibiliza. Como nosso objetivo é desenvolver uma dashboard personalizada para exibir as informações dos visitantes do condomínio e implementar funcionalidades específicas, a partir de agora, trabalharemos para desenvolver os templates que irão dispôr as informações necessárias e funcionalidades específicas! 

Sendo assim, posso dizer que a partir de agora as coisas começam a ficar mais interessantes!

Neste próximo módulo, aprenderemos a configurar o Django para trabalhar com arquivos estáticos \(CSS e JS\) e templates HTML.

{% hint style="warning" %}
O Django, por padrão, vem configurado para que já seja possível trabalhar com templates HTML, mas vamos alterar as configurações para que a haja uma maior organização dos arquivos
{% endhint %}

### Criando a pasta templates em nosso projeto

Sendo um framework web, o Django precisa fornecer uma maneira de gerar os templates de forma dinâmica, afim de exibir valores específicos para atender os diversos cenários. Essencialmente, um template é constituído por uma parte estática e uma parte onde serão exibidas as informações desejadas, sendo necessário seguir uma sintaxe específica para exibição de valores e funções fornecidas pelo framework. O Django nos fornece uma _engine_ rica e poderosa capaz de executar funções condicionais, loops, exibir valores e várias outras funcionalidades diretamente nos templates HTML através de tags.

{% hint style="info" %}
Uma engine de template nada mais é que uma aplicação que visa facilitar o processo de criação de templates HTML dinâmicos e tornar o processo de envio e exibição de informações nos templates menos burocrático
{% endhint %}

Por padrão, o Django vem configurado para procurar os templates dentro de cada aplicativo. Isto é, em cada aplicativo deverá existir uma pasta **templates** para armazenar os templates referentes ao aplicativo em questão. Todavia, para uma melhor organização, utilizaremos uma pasta externa para armazenar os arquivos de templates do projeto.

Para isso, começaremos alterando o arquivo `settings.py` do nosso projeto. Nesse arquivo é possível encontrar a variável `TEMPLATES`, que é responsável por definir as configurações de template do Django, como _engine_ a ser utilizada, diretórios que armazenam os templates, dentre outras.

A variável `TEMPLATES` é uma lista que recebe um dicionário contendo valores específicos, tais como `BACKEND`, `DIRS`, `APP_DIRS` e `OPTIONS`, cada um com uma função específica. No nosso caso, vamos alterar o valor `DIRS` de uma lista vazia para uma lista contendo a string "templates", que é o nome da pasta que utilizaremos para armazenar nossos templates na raíz do projeto.

Como definimos que os templates devem ser buscados na pasta **templates**, podemos apagar o valor `APP_DIRS` do dicionário de configuração, pois esta opção diz para o Django buscar os templates em cada aplicativo.

Nossa variável `TEMPLATES` ficará da seguinte forma:

```python
TEMPLATES = [
    {
        "BACKEND": "django.template.backends.django.DjangoTemplates",
        "DIRS": ["templates"],
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

{% file src="../.gitbook/assets/templates.zip" caption="Iniciar o download" %}

### Criando a pasta static em nosso projeto

Feito isso, vamos agora definir as configurações para os arquivos estáticos do nosso projeto. Assim como para os templates, o Django também nos dá toda a estrutura necessária para trabalharmos com arquivos estáticos.

Por "arquivos estáticos", entenda arquivos do tipo CSS, JS \(javascript\) e imagens que serão utilizadas em nossos templates, tais como logotipo, imagem padrão para avatar de usuários, dentre outras.

Para realizarmos a configuração, vamos novamente alterar o arquivo `settings.py.` Ao final do arquivo, adicione o seguinte trecho de código para definir a URL onde os arquivos estáticos serão armazenados:

```python
STATIC_URL = "/static/"
```

Novamente, para facilitar e economizar tempo, você pode fazer o download da pasta zipada clicando no link abaixo \(agora da pasta **static**, claro\) e colocá-la na raiz do projeto:

{% file src="../.gitbook/assets/static.zip" caption="Iniciar o download" %}

Faça o download e coloque a pasta **static** na raíz do projeto, juntamente com a pasta **templates**. Feito isso, já podemos utilizar templates HTML e arquivos estáticos em nosso projeto!

## Criando views que renderizam templates

Falamos bastante sobre templates HTML, mas o que é HTML, afinal?

O HTML é uma das linguagens que são utilizadas para o desenvolvimento de sites na web. HTML significa **Hypertext Markup Language**, que ****em português quer dizer **Linguagem de Marcação de Hipertexto**. 

Como o Django já nos dá tudo que é necessário para criarmos aplicações web, ele também nos dá a possibilidade de criarmos views que renderizam templates. Isto é, a partir de agora, ao invés de ser exibido um texto ao acessarmos uma URL através do navegador, como fizemos anteriormente, vamos dizer para o Django que é necessário exibir um template HTML, afim de exibir as informações de forma estruturada e de modo que fiquei fácil a compreensão para nossos usuários.

### Conhecendo a função render

Para que o Django exiba um template ao invés de um texto em tela, precisaremos alterar o retorno da nossa view denominada `index`. Até então, utilizamos o `HttpResponse` para retornar uma mensagem, mas a partir de agora utilizaremos a função `render` que é responsável por combinar o template com informações



## Entendendo as adaptações realizadas no template

Entendendo as adaptações realizadas no template para trabalhar com a engine do Django... index.html

## Exibindo variáveis no template




# Módulo 04 - in dev

## Configurando a aplicação para trabalhar com arquivos estáticos e templates HTML

Nos capítulos anteriores, criamos toda a estrutura necessária para criação de usuários do sistema e porteiros que serão os responsáveis por operar a dashboard proposta. Além disso, também preparamos o ambiente de desenvolvimento e aprendemos bastante sobre detalhes técnicos do funcionamento do Django.

Trabalhamos bastante nos arquivos `models.py` e conhecemos o poder existente do Admin do Django, todavia, como o nosso objetivo é desenvolver uma dashboard personalizada para exibir as informações dos visitantes do condomínio e implementar funcionalidades específicas, será necessário desenvolver os templates para podermos dispor as informações necessárias.

Desta forma, neste próximo módulo, aprenderemos a configurar o Django para trabalhar com arquivos estáticos \(CSS e JS\) e templates HTML.

{% hint style="warning" %}
O Django, por padrão, vem configurado para que já seja possível trabalhar com templates HTML, mas vamos alterar as configurações para que a haja uma maior organização dos arquivos
{% endhint %}

## Criando a pasta templates em nosso projeto

Sendo um framework web, o Django precisa fornecer uma maneira de gerar os templates de forma dinâmica, afim de exibir valores específicos para atender os diversos cenários. Essencialmente, um template é constituído por uma parte estática e uma parte onde serão exibidas as informações desejadas, sendo necessário seguir uma sintaxe específica para exibição de valores e funções fornecidas pelo framework. O Django nos fornece uma _engine_ rica e poderosa capaz de executar funções condicionais, loops, exibir valores e várias outras funcionalidades diretamente nos templates HTML através de tags.

Por padrão, o Django vem configurado para procurar os templates dentro de cada aplicativo. Isto é, em cada aplicativo deverá existir uma pasta "templates" para armazenar os templates referentes ao aplicativo em questão. Todavia, para uma melhor organização, utilizaremos uma pasta externa para armazenar os arquivos de templates do projeto

Para isso, começaremos alterando o arquivo `settings.py` do nosso projeto. Nesse arquivo é possível encontrar a variável `TEMPLATES`, que é responsável por definir as configurações de template do Django, como engine a ser utilizada, diretórios que armazenam os templates, dentre outras.

A variável `TEMPLATES` é uma lista que recebe um dicionário contendo valores específicos, tais como `BACKEND`, `DIRS`, `APP_DIRS` e `OPTIONS`, cada um com uma função específica. No nosso caso, vamos alterar o valor `DIRS` de uma lista vazia para uma lista contendo a string "templates", que é o nome da pasta que utilizaremos para armazenar nossos templates. Como definimos que os templates devem ser buscados na pasta "templates" na raíz do nosso projeto, podemos apagar o valor `APP_DIRS` do dicionário de configuração.

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

Feito isso, podemos criar a pasta templates na raíz do nosso projeto. 



## Entendendo as adaptações realizadas no template 

Entendendo as adaptações realizadas no template para trabalhar com a engine do Django

## Criando views que renderizam templates

Exibir funcionando

## Exibindo variáveis no template




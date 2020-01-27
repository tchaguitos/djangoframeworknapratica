# Módulo 02

## Iniciando seu primeiro projeto Django

Conforme visto, vamos desenvolver uma dashboard para controle de visantes, mas, antes disto, temos que iniciar nosso projeto.  
  
Felizmente, o Django já nos dá alguns scripts que servem para executar tarefas específicas do nosso projeto e, dentre elas, criar o esqueleto de um novo projeto. Ao fim da execução do script, teremos algumas pastas e arquivos que deverão ser alterados conforme nossa necessidade.  
  
Para iniciar um novo projeto Django, vamos ativar nosso ambiente virtual e utilizar o comando `startproject`. Para ativar o ambiente virtual, basta acessar a pasta do projeto \(`controle-visitantes`\) e utilizar o comando `source`:

```python
$ source env/bin/activate
```

Após a ativação do ambiente, vamos criar um novo projeto utilizando o `django-admin`:

```text
(env)$ django-admin startproject controle_visitantes .
```

{% hint style="info" %}
Estamos iniciando nosso projeto com nome `controle_visitantes` na pasta em que estamos trabalhando, isto é, na pasta `controle-visitantes` \(note o ponto especificando o diretório atual\). O comando `startproject` pode receber, além do nome que o projeto terá, o diretório em que o mesmo deverá ser iniciado \(caso o diretório não seja informado, o Django criará um diretório de mesmo nome do projeto\). Além disso, o Django permite apenas letras, números e o underline em nomes de projetos.
{% endhint %}

O `django-admin` é um pacote de comandos úteis para tarefas administrativas dentro da aplicação e dispõe de comandos para criar o projeto, iniciar o servidor de desenvolvimento, verificar erros e muito mais.  
  
Como o Django já possui um servidor de desenvolvimento integrado que nos possibilita rodar a aplicação localmente, vamos iniciá-lo e verificar se está tudo funcionando conforme esperado. Para isso, basta utilizar o seguinte comando:

```text
(env)$ python manage.py runserver
```

O `manage.py` funciona exatamente como o `django-admin`. Na verdade, eles são a mesma coisa, com a diferença que que o `manage.py`, por baixo dos panos, define um arquivo de configurações para ser utilizado pelo servidor de desenvolvimento.

{% hint style="warning" %}
Por enquanto, vamos ignorar os avisos referentes às migrações no banco de dados. Em breve vamos entender melhor do que se trata.
{% endhint %}

## Entendendo a estrutura do projeto

Como vimos anteriormente, o `django-admin` cria o esqueleto de um novo projeto e é isso que nós vamos entender agora: a estrutura que foi criada.

Abaixo temos a lista de todos os diretórios e arquivos que foram criados pelo `django-admin`. Vamos passar por cada um deles e entender o motivo pelo qual estão aí. A lista é a seguinte:

```text
controle-visitantes/    
    manage.py    
    controle_visitantes/
        __init__.py
        settings.py
        urls.py
        wsgi.py
```

* O primeiro diretório nomeado `controle-visitantes/` é apenas um container para armazenar os arquivos do nosso projeto. O nome do diretório não é importante para o Django e pode ser alterado a qualquer momento. 
* O arquivo `manage.py` é um utilitário de linha de comando que permite que a gente interaja com o projeto Django de diversas maneiras, conforme vimos anteriormente. 
* O segundo diretório nomeado `controle_visitantes/` é o diretório que agrupa o pacote Python referente ao nosso projeto. O nome do diretório é o nome do nosso pacote e ele é importante pois nos auxilia no processo de importação de arquivos e funções. Sendo assim, alterar o nome deste diretório nos trará sérios problemas. 
* O arquivo `controle_visitantes/__init__.py` é um arquivo vazio que diz ao Python que aquele diretório deve ser reconhecido e tratado como um pacote Python. 
* O arquivo `controle_visitantes/settings.py` é o arquivo de configurações do nosso projeto Django. Nele nós podemos detalhar como o projeto funciona e quais definições estão disponíveis. 
* O arquivo `controle_visitantes/urls.py` é o arquivo que declara as URLs do nosso projeto. Para uma URL ser acessível através do nosso projeto, temos que declará-la neste arquivo. 
* O arquivo `controle_visitantes/wsgi.py` é um ponto de integração para servidores web que implementam o WSGI, um padrão Python que descreve como os servidores devem se comunicar com as aplicações web.

## Criando nosso primeiro aplicativo Django

Agora que o ambiente está configurado, é hora da gente começar a colocar a mão na massa!

Conforme vimos, um projeto Django nada mais é que um pacote Python que deve seguir algumas convenções, como nomenclatura de arquivos e diretórios. O mesmo, obviamente, se estende para os aplicativos deste projeto. Sendo assim, devemos seguir uma estrutura básica dentro dos nossos aplicativos, que nada mais são que outros pacotes Python. A diferença básica entre eles é que um aplicativo deve executar uma tarefa em específico, como o gerenciamento de usuários do sistema, quando um projeto é, na verdade, um conjunto de configurações e aplicativos que executam tarefas distintas. Um projeto pode ter vários aplicativos e um aplicativo pode estar presente em vários projetos.

Felizmente, podemos usar o nosso bom e velho amigo `manage.py` para nos auxiliar nessa tarefa de criar toda a estrutura necessária para um novo aplicativo. Vamos iniciá-lo utilizando o comando abaixo:

```text
(env)$ python manage.py startapp usuarios
```

Após o comando ser executado, uma nova pasta, com o nome escolhido - usuarios, será criada dentro do projeto. Essa pasta terá a seguinte estrutura, que vamos entender melhor no decorrer das aulas:

```text
usuarios/
    __init__.py
    admin.py
    apps.py
    migrations/
        __init__.py
    models.py
    tests.py
    views.py
```

Todos os aplicativos do seu projeto Django terão essa mesma estrutura devido às convenções e padrões que falamos anteriormente. Os padrões são legais pois nos ajudam a manter uma certa organização, o que nos facilita bastante em casos de manutenções e deixa nosso código mais previsível.

Antes de começarmos a trabalhar no código de um novo aplicativo, temos que registrá-lo nas configurações do nosso projeto para que o mesmo seja reconhecido. Caso esse passo não seja executado, o projeto não saberá o que é o aplicativo `usuarios`.

Para isso, vamos abrir o arquivo `settings.py` no diretório principal do projeto e procurar pela variável `INSTALLED_APPS`. Essa variável guarda o nome dos aplicativos que são utilizados no projeto. Por padrão, o Django já começa utilizando alguns aplicativos do próprio framework, como `admin`, `auth`, `sessions` e `messages`, cada um com uma finalidade específica.

```python
INSTALLED_APPS = [
    "django.contrib.admin",
    "django.contrib.auth",
    "django.contrib.contenttypes",
    "django.contrib.sessions",
    "django.contrib.messages",
    "django.contrib.staticfiles",
]
```

Para registrar nosso aplicativo, basta colocarmos ele no final dessa lista e o Django fará todo o resto. Para manter uma organização melhor, vamos separar os aplicativos do Django dos nossos aplicativos nessa configuração. Para isso, basta inserir o código abaixo logo após a variável `INSTALLED_APPS`. 

```python
INSTALLED_APPS += [
    "usuarios",
]
```

Note que estamos utilizando um operador de atribuição diferente, sendo `+=` ao invés de `=`. Esse operador faz com que o valor existente na variável seja mantido e a gente acrescente o valor à direita do operador. Isto é, estamos mantendo os aplicativos do Django e adicionando os nossos.

O código que teremos será o seguinte:

```python
INSTALLED_APPS = [
    "django.contrib.admin",
    "django.contrib.auth",
    "django.contrib.contenttypes",
    "django.contrib.sessions",
    "django.contrib.messages",
    "django.contrib.staticfiles",
]

INSTALLED_APPS += [
    "usuarios",
]
```

## Criando nosso "Hello World"

O "Hello world" é aquele famoso programa de computador que imprime na tela a frase "Hello world" e é sempre utilizado como um exemplo minimalista de determinada linguagem ou framework. Não vamos deixar a tradição de lado e vamos implementar o nosso "Hello world" em Django. A diferença é que, como estamos trabalhando na web, vamos exibir nosso "Hello world" diretamente acessando a URL que vamos configurar. Mão na massa!

Vamos começar trabalhando no arquivo `views.py` do nosso aplicativo `usuarios`. A camada view, como falamos, é a responsável por encapsular a lógica que recebe e responde as requisições dos nossos usuários.

Toda view no Django é uma função de retorno vinculada a uma URL específica. Sendo assim, não existe uma URL sem uma função de view.

```python
from django.shortcuts import render
from django.http import HttpResponse

def index(request):
    return HttpResponse("Hello world")
```

Essa é o exemplo mais básico de view que podemos escrever no Django. Ela apenas retorna um objeto do tipo `HttpResponse` que nada mais é, neste caso, que um texto simples.

Com a nossa função de view pronta, temos agora que mapear ela para ser chamada junto à uma URL. Para isso, vamos alterar o arquivo `urls.py` no diretório principal do nosso projeto. O arquivo terá um conteúdo parecido com o abaixo:

```python
from django.urls import path
from django.contrib import admin

urlpatterns = [
    path("admin/", admin.site.urls),
]
```

O primeiro passo será importar o arquivo de views do nosso aplicativo `usuarios` e depois adicionar uma nova linha na lista `urlpatterns`.

Sendo assim, vamos substituir o conteúdo pelo seguinte código:

```python
from django.urls import path
from django.contrib import admin

import usuarios.views

urlpatterns = [
    path("admin/", admin.site.urls),
    
    path(
        "",
        usuarios.views.index,
        name="index"
    ),
]
```

A função path pode receber uma string que será a URL a ser acessada, uma função a ser executada e um nome para a URL ser identificada mais facilmente dentro do projeto. O nome da view é bem útil para casos onde temos que renderizar o endereço completo da URL no template ou direcionar o usuário para uma página específica, por exemplo. 

Feito isso, vamos utilizar o comando para iniciar nosso servidor de desenvolvimento e ver o que aparece ao acessarmos o endereço através do navegador.

```bash
(env)$ python manage.py runserver
```

Feito isso e não havendo erros no terminal, você deverá acessar o endereço `http://127.0.0.1:8000/` em seu navegador e visualizar nosso tão esperado "Hello world".

## Escrevendo as models

### Escrevendo a classe modelo

Nossa camada _model_ nada mais é que uma representação exata do nosso banco de dados, sendo a classe uma tabela e seus atributos os campos dessa tabela. É nessa camada que guardamos as informações que serão disponibilizadas para outras camadas. Como o Django segue o princípio DRY \(_don't repeat yourself_\), o objetivo é definir o modelo de dados em um único lugar e automaticamente derivar informações a partir dele.

O aplicativo `usuarios` será responsável por gerenciar os usuários do sistema. Ou seja, como vamos salvar as credenciais, quais credenciais vamos utilizar e tudo mais. Nesse ponto, é importante dizer que o Django já fornece um modelo padrão para usuários do sistema muito bom e útil para diversos cenários, mas que não dispõe de um suporte amigável para customizações. Se, por exemplo, precisarmos utilizar um e-mail ao invés de um nome de usuário para acessar o sistema, encontraríamos problemas com o modelo fornecido por padrão.

O próprio Django, em sua documentação, cita exemplos em que o modelo padrão não é o mais apropriado e recomenda alternativas para contornar o problema. Uma das alternativas é sobrescrever o modelo padrão por outro que será definido por nós. Para isso, começaremos alterando o arquivo `models.py` dentro do diretório do nosso aplicativo, denominado `usuarios`.

Ao abrir o arquivo, vamos apagar o conteúdo que foi colocado pelo Django e começar importando as classes `BaseUserManager`, `AbstractUser` e `PermissionsMixin`. Estas classes vão nos auxiliar na tarefa de criar um novo modelo para usuários em nosso projeto.

```python
from django.db import models
from django.contrib.auth.models import (
    BaseUserManager,
    AbstractBaseUser,
    PermissionsMixin,
)
```

Vamos começar criando nossa classe `Usuario` como subclasse de `AbstractBaseUser` e `PermissionsMixin` com os atributos `e-mail` e `tipo de usuário`, sendo o primeiro um campo do tipo e-mail e o segundo apenas um campo de texto simples.

Nosso modelo terá suporte para diferentes tipo de usuário, sendo este tipo definido pelo campo `tipo_usuario`. Este campo é do tipo texto e deverá aceitar somente 1 caractere. Por hora, teremos apenas o tipo "porteiro" que será definido pela letra `P` e vamos colocá-lo como valor padrão no nosso atributo. Ou seja, caso a gente não especifique, o tipo de usuário que será criado será do tipo "porteiro". 

{% hint style="info" %}
Mais a frente, esse tipo será importante para definirmos páginas que porteiros podem ou não acessar. Ou para criarmos tipos de usuários diferentes para acessar uma dashboard com informações diferentes...
{% endhint %}

```python
class Usuario(AbstractBaseUser, PermissionsMixin):

    email = models.EmailField(
        verbose_name="E-mail do usuário",
        max_length=254,
        unique=True,
    )
    
    tipo_usuario = models.CharField(
        verbose_name="Tipo de usuário",
        max_length=1,
        default="P",
    )
```

{% hint style="info" %}
Existem inúmeros tipos de campos que o Django traz por padrão, sendo eles classes contidas no pacote models do Django. EmailField e CharField são apenas alguns exemplos, no decorrer do curso vamos conhecer mais deles.
{% endhint %}

Para delimitar as opções referentes ao tipo do usuário, utilizaremos o argumento `choices` que pode ser passado para campos do tipo texto simples \(_CharField_\). O argumento deve ser passado junto de uma tupla, uma estrutura de dados conhecida no mundo Python. Vamos criar uma variável chamada `TIPO_USUARIO` que será uma tupla com a opção `P` e o label para a opção que será "Porteiro" e adicionar o argumento ao campo `tipo_usuario`.

```python
class Usuario(AbstractBaseUser, PermissionsMixin):
    
    TIPO_USUARIO = (
        ("P", "Porteiro"),
    )

    email = models.EmailField(
        verbose_name="E-mail do usuário",
        max_length=254,
        unique=True,
    )
    
    tipo_usuario = models.CharField(
        verbose_name="Tipo de usuário",
        max_length=1,
        choices=TIPO_USUARIO
        default="P",
    )
```

Após definirmos estes campos, vamos definir alguns que são obrigatórios para um modelo de usuário do Django. Os campos `is_active`, `is_staff` e `is_superuser` são herdados das classes `AbstractBaseUser` e `PermissionsMixin`, todavia, vamos defini-los aqui pois vamos alterar o comportamento padrão de algum deles.

Vamos aproveitar também para criar a variável `USERNAME_FIELD` que é quem especifica para o Django qual campo deve ser utilizado como nome de usuário. 

```python
class Usuario(AbstractBaseUser, PermissionsMixin):
    
    TIPO_USUARIO = (
        ("P", "Porteiro"),
    )

    email = models.EmailField(
        verbose_name="E-mail do usuário",
        max_length=254,
        unique=True,
    )
    
    tipo_usuario = models.CharField(
        verbose_name="Tipo de usuário",
        max_length=1,
        choices=TIPO_USUARIO,
        default="P",
    )
    
    is_active = models.BooleanField(
        "usuario ativo?",
        default=False
    )
        
    is_staff = models.BooleanField(
        "usuario é da equipe de desenvolvimento?",
        default=False
    )

    is_superuser = models.BooleanField(
        "usuario é um superusuário?",
        default=False
    )
    
    USERNAME_FIELD = "email"
    
    class Meta:
        verbose_name="Usuário"
        verbose_name_plural = "Usuários"
        db_table = "usuario"
    
    def __str__(self):
        return self.email
```

Após a definição destes campos e variáveis, vamos escrever uma classe interna à classe `Usuario` chamada `Meta`. Essa classe é comum a todos os modelos e serve para que a gente informe ao Django metadados. Existem diversas opções de metadados que podemos explicitar mas, por hora, vamos nos concentrar no `verbose_name`, `verbose_name_plural` e `db_table.` Estas opções deixam claro para o Django como ele deve apelidar o model, qual o apelido no plural e o nome da tabela referente ao model criado. 

O método `__str__` é obrigatório e chamado sempre que transformamos o objeto numa string para fins de exibição. Um dos casos em que isso ocorre é quando o Django precisa exibir a instância no _Django Admin_ \(não se preocupe com esse nome agora, vamos conhecê-lo melhor em breve\). Sendo assim, seu método `__str__` deve sempre retornar um texto fácil e de rápida identificação para seres humanos. Feito isso, podemos partir para o segundo passo!

### Escrevendo um manager personalizado

O segundo passo para substituirmos o modelo de usuários padrão do Django é criarmos uma subclasse de `BaseUserManager` e sobrescrevermos os métodos `create_user` e `create_superuser`. Estes métodos são responsáveis por criar usuários e super usuários em nosso sistema e devem ser sobrescritos para se adequarem às nossas necessidades. Devido ao fato de estarmos estabelecendo uma relação de herança entre `UsuarioManager` e `BaseUserManager`, não precisamos implementar todos os atributos e métodos, pois estas informações são repassadas à classe filho. 

Uma classe _manager_ é uma interface que fornece informações sobre e como as queries devem ser executadas pela classe modelo quando houver interação com o banco de dados. Para cada classe modelo, existe pelo menos um _manager_.

Vamos escrever nossa classe _manager_ em cima da classe `Usuario`:

```python
class UsuarioManager(BaseUserManager):

    def create_user(self, email, tipo_usuario, password=None):
        user = self.model(
            email=self.normalize_email(email),
            tipo_usuario=tipo_usuario,
        )

        user.is_active = False
        user.is_staff = False
        user.is_superuser = False

        if password:
            user.set_password(password)

        user.save(using=self._db)

        return user
    
    def create_superuser(self, email, password):
        user = self.create_user(
            email=self.normalize_email(email),
            password=password,
            tipo_usuario="P",
        )
        
        user.is_active = True
        user.is_staff = True
        user.is_superuser = True
        
        user.set_password(password)
        user.save(using=self._db)
        
        return user

class Usuario(models.Model):
    # código omitido...
```

Com a classe e métodos escritos, agora temos um _manager_ com a função de criar usuários conforme a nossa necessidade. Com isso, nosso sistema está quase pronto para criar usuários, faltando apenas explicitar que a classe modelo deve utilizar este _manager_ como padrão. Para isso, basta apenas adicionarmos um atributo na classe modelo com o nome do manager que queremos utilizar.

O Django utiliza o nome "objects" para o manager padrão da classe, sendo assim, vamos apenas sobrescrever o manager padrão pelo que nós criamos. Para isso, vamos criar o atributo `objects` na classe `Usuario` atribuindo a ele a classe `UsuarioManager`.

```python
class Usuario(AbstractBaseUser, PermissionsMixin):
    
    TIPO_USUARIO = (
        ("P", "Porteiro"),
    )

    email = models.EmailField(
        verbose_name="E-mail do usuário",
        max_length=254,
        unique=True,
    )
    
    tipo_usuario = models.CharField(
        verbose_name="Tipo de usuário",
        max_length=1,
        choices=TIPO_USUARIO
        default="P",
    )
    
    is_active = models.BooleanField(
        "usuario ativo?",
        default=False
    )
        
    is_staff = models.BooleanField(
        "usuario é da equipe de desenvolvimento?",
        default=False
    )

    is_superuser = models.BooleanField(
        "usuario é um superusuário?",
        default=False
    )
    
    USERNAME_FIELD = "email"
    
    objects = UsuarioManager()
    
    class Meta:
        verbose_name="Usuário"
        verbose_name_plural = "Usuários"
        db_table = "usuario"
    
    def __str__(self):
        return self.email
```

Com isso, estamos prontos para efetuar as alterações no nosso banco de dados e criarmos nosso primeiro usuário!

### Alterando o modelo padrão nas configurações

Para dizermos ao Django que ele deve utilizar a nossa classe modelo ao invés do modelo padrão para usuários do sistema, temos que adicionar a variável `AUTH_USER_MODEL` ao arquivo `settings.py` no diretório principal do nosso projeto e apontar para a classe a ser utilizada. 

```python
AUTH_USER_MODEL = "usuarios.Usuario"
```

Ao escrevermos isso, estamos dizendo "hey, Django, use a classe Usuario do aplicativo usuarios como modelo de usuários do sistema".

### Criando as tabelas do nosso banco de dados

Com o trecho de código que escrevemos o Django já é capaz de executar instruções para criação da tabela `usuario` no nosso banco de dados e disponibilizar uma API de acesso aos objetos do tipo `Usuario,` mas antes precisamos avisar ao Django a respeito destas alterações. Sempre que ocorrer alguma alteração nos modelos ou você criar um novo modelo, é necessário avisar ao Django para que ele cuide de toda a parte anterior à efetivação das mudanças no banco de dados.

Para isso, quando escrevemos uma classe modelo, temos que executar o comando `makemigrations`. Esse comando vai criar um arquivo de migração contendo todas as alterações que devem ser feitas no banco de dados, tais como criação de tabelas com determinados atributos, alteração nos atributos, dentre outras. Para isso, basta executar o seguinte comando:

```bash
(env)$ python manage.py makemigrations usuarios
```

Ao executar o comando `makemigrations`, você está dizendo ao Django para armazenar as alterações realizadas em forma de _migração_. Uma _migração_ nada mais é que um arquivo de texto contendo todos os passos que devem ser executados para efetivação das alterações no banco de dados. Aparecerá algo como isso na tela do terminal:

```python
Migrations for 'usuarios':
  usuarios/migrations/0001_initial.py
    - Create model Usuario
```

Existe também um comando para rodar as migrações e gerenciar o _schema_ do banco de dados de forma automática. O comando `migrate` é quem vai reunir todas as migrações que ainda não foram executadas e aplicar elas em seu banco de dados - isto é, vai sincronizar seu banco de dados com as informações que estão na classe modelo. Para efetuar as alterações em nosso banco de dados vamos executar o comando:

```bash
(env)$ python manage.py migrate
```

Migrações são um recurso poderoso pois nos permitem alterar as classes modelos ao longo do tempo, sem a necessidade de manipular nosso banco de dados. O comando `migrate` é especialista em atualizar nosso banco de dados em tempo real sem perder dados.

```bash
Operations to perform:
  Apply all migrations: admin, auth, contenttypes, sessions, usuarios

Running migrations:
  Applying contenttypes.0001_initial... OK
  Applying contenttypes.0002_remove_content_type_name... OK
  Applying auth.0001_initial... OK
  Applying auth.0002_alter_permission_name_max_length... OK
  Applying auth.0003_alter_user_email_max_length... OK
  Applying auth.0004_alter_user_username_opts... OK
  Applying auth.0005_alter_user_last_login_null... OK
  Applying auth.0006_require_contenttypes_0002... OK
  Applying auth.0007_alter_validators_add_error_messages... OK
  Applying auth.0008_alter_user_username_max_length... OK
  Applying auth.0009_alter_user_last_name_max_length... OK
  Applying auth.0010_alter_group_name_max_length... OK
  Applying auth.0011_update_proxy_permissions... OK
  Applying usuarios.0001_initial... OK
  Applying admin.0001_initial... OK
  Applying admin.0002_logentry_remove_auto_add... OK
  Applying admin.0003_logentry_add_action_flag_choices... OK
  Applying sessions.0001_initial... OK
```

FALAR SOBRE OS RESULTADOS DA MIGRAÇÃO NO TERMINAL...

Agora, vamos lembrar os passos que devemos seguir para efetuar as alterações em nosso banco de dados:

1. Realizar alterações nos arquivos `models.py`
2. Executar `python manage.py makemigrations` para criar migrações das modificações
3. Executar `python manage.py migrate` para aplicar as modificações no banco de dados

Feito isso, temos nosso modelo personalizado de usuários criado e ativado em nosso banco de dados e estamos prontos para o próximo passo!

## Criando um super usuário

A gente viu que o Django segue uma filosofia que tenta aproveitar o máximo de coisas para evitar que a gente fique repetindo código. Pensando nisso, os desenvolvedores do framework disponibilizam um painel administrativo com funções para adicionar, alterar e deletar conteúdo com base nas classes modelos do nosso projeto. Como num passe de mágica!

O Django foi desenvolvido em um ambiente de redação, onde havia uma clara separação entre “produtores de conteúdo” e o site “público”. Gerentes utilizam o sistema para adicionar notícias, eventos, resultado de esportes, por exemplo, e o conteúdo é exibido no site público. O Django soluciona o problema de criar uma interface unificada para os administradores editarem o conteúdo.

Como a administração do Django não foi desenvolvida para ser utilizada pelos visitantes do site, mas sim pelos gerentes, temos que criar um tipo diferente de usuário, que seria nosso "super usuário". Para isso basta utilizarmos o comando `createsuperuser` do nosso bom e velho amigo `manage.py`:

```text
(env)$ python manage.py createsuperuser
```

Ao executar o comando, nosso terminal ficará travado esperando que a gente informe o e-mail a ser utilizado pelo super usuário. Após o e-mail, temos que definir a senha e repetir a senha. Não ocorrendo erros, temos o nosso super usuário criado e pronto para acessar o painel administrativo do Django.

![](../.gitbook/assets/captura-de-tela-em-2019-11-25-23-50-37.png)

Vamos iniciar novamente nosso servidor de desenvolvimento e acessar o endereço `http://127.0.0.1:8000/admin/` em nosso navegador e vamos nos deparar com a tela de login do Djando Admin.

![](../.gitbook/assets/captura-de-tela-em-2019-11-25-23-42-34.png)

Utilize e-mail e senha informados na hora da criação do super usuário \(`python manage.py createsuperuser`\) para acessar e, dando tudo certo, você será direcionado para a página inicial da administração do Django:

![](../.gitbook/assets/captura-de-tela-em-2019-11-25-23-43-37.png)

## Conhecendo o Django Admin

Ao acessarmos o painel administrativo, é possível notar que nosso aplicativo de usuários não está sendo exibido. Isso porque para que nossas classes modelos sejam identificadas pelo painel administrativo é necessário alterar o arquivo `admin.py`. Esse arquivo sempre existirá dentro de um aplicativo criado utilizando o `manage.py` e é quem irá dizer ao site de administração que nossa classe deve ter uma interface de administração. 

Vamos abrir o arquivo `usuarios/admin.py` e importar nossa classe `Usuario`:

```python
from django.contrib import admin
from usuarios.models import Usuario
```

Após a importação, temos apenas que registrar a classe no admin. Para isso, basta utilizar `admin.site.register` passando a classe `Usuario` como argumento:

```python
from django.contrib import admin
from usuarios.models import Usuario

admin.site.register(Usuario)
```

Feito isso, vamos voltar ao painel administrativo do Django e podemos notar que agora existe uma seção referente ao nosso aplicativo usuarios. O mais legal é que os nomes que definimos em `verbose_name` e `verbose_name_plural` são utilizados aqui, além do valor que definimos como retorno do método `__str__`.

## Alterações necessárias nas configurações

Nosso projeto já está rodando sem problemas e possui um modelo personalizado de usuários. Isso já é bem legal, mas ainda vamos realizar algumas alterações nas configurações para que ele se adeque ainda mais às nossas necessidades. Neste momento, vamos alterar configurações de fuso horário e idioma utilizado.

O Django possui integrado um módulo para internacionalização e localização. Esses módulos são interessantes pois permitem que a aplicação funcione em idiomas e formatos diferentes com base nas preferências do usuário. Um exemplo de funcionamento do módulo de internacionalização é o próprio painel administrativo do Django que funciona dessa maneira.  

Para alterar o idioma padrão utilizado no projeto, vamos no arquivo `settings.py` e procurar pela variável `LANGUAGE_CODE`. Essa variável recebe uma string referente ao identificador do idioma e país de origem com base na especificação que define os formatos de linguagens para serem utilizados. Para nosso caso, utilizaremos a string `pt-BR` que é quem diz para o Django: "hey, cara, utilize português do Brasil como idioma principal do projeto". Para fazer isso, basta que a gente substitua o valor da variável pelo seguinte valor:

```python
LANGUAGE_CODE = "pt-BR"
```

Com isso, todas as mensagens e textos exibidos no painel administrativo já serão traduzidos automaticamente pelo módulo de internacionalização.

![](../.gitbook/assets/captura-de-tela-em-2019-11-26-00-07-41.png)

Após isso, nosso próximo passo será alterar o fuso horário padrão do projeto. Essa configuração é importante pois não queremos que datas erradas sejam exibidas para nossos usuários. Por padrão, o Django trabalha e exibe as datas no fuso horário `America/Chicago`, mas como esse não é o fuso horário para a nossa região, vamos inserir a configuração correta para nós. Existe também uma lista de fuso horários disponíveis e como podemos especificar eles através de uma string simples, como acontece no caso dos identificadores de linguagens e países. Para nosso caso, utilizarmos o fuso horário `America/Sao_Paulo`. Ainda no arquivo `settings.py,` vamos encontrar a variável `TIME_ZONE` e alterar seu valor para o da nossa região:

{% hint style="success" %}
Antes de realizar a modificação, observe os horários no painel administrativo do Django. O atributo "último login" e o histórico de modificações do usuário aparecem em um horário que não é o nosso.
{% endhint %}

```python
TIME_ZONE = "America/Sao_Paulo"
```

Com isso, além do nosso projeto possuir um modelo personalizado de usuários, agora ele exibe mensagens e horários no idioma e fuso horário que é o mais apropriado para nossa região.

A partir de agora as coisas vão ficando mais interessantes e o projeto começa a tomar forma... então, já que aprendemos bastante, bora aprender muito mais no próximo capítulo!



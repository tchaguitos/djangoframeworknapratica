# Capítulo 03

## Criando aplicativo para gerenciar porteiros

Aprendemos como instalar nossas dependências, como iniciar um novo projeto e já até escrevemos um modelo personalizado de usuários. Com isso, podemos dizer que já temos uma base sólida para iniciar, de fato, a construção dos módulos que estarão diretamente ligados aos requisitos que o sistema deverá atender.

A partir dos requisitos que temos, é possível identificar que existe a necessidade de que os porteiros do condomínio tenham acesso a uma página para registrar os visitantes. Com isso em mente, podemos concluir:

* Deverá existir um modelo para representar os porteiros do condomínio
* Os porteiros deverão ter acesso a uma dashboard com funcionalidades específicas
* Deverá existir uma página para um porteiro registrar um visitante no condomínio

Vamos nos concentrar em cada item separadamente e, por agora, estaremos focados em desenvolver um modelo que represente nossos porteiros. 

Já falamos que aplicativos Django devem executar tarefas específicas, como gerenciar os usuários do sistema - que é exatamente o que o nosso aplicativo `usuarios` faz. Sendo assim, vamos iniciar um novo aplicativo com nome de "porteiros" que será responsável por gerenciar tudo referente aos porteiros do sistema. Para isso, vamos utilizar nosso bom e velho amigo `manage.py`:

```text
(env)$ python manage.py startapp porteiros
```

Conforme vimos, será criado um diretório com o nome informado para representar o módulo do nosso aplicativo e, além disso, temos que instalar nosso novo aplicativo nas configurações do projeto. Vamos abrir nosso arquivo de configurações e inserir o aplicativo "porteiros" na variável `INSTALLED_APPS`:

```python
INSTALLED_APPS += [
    "usuarios",
    "porteiros",
]
```

{% hint style="info" %}
Caso não se lembre, tudo bem... nosso arquivo de configurações é o `settings.py`.
{% endhint %}

Agora que criamos e registramos nosso aplicativo, vamos partir para a construção dos modelos que vão representar nossos porteiros.

## Escrevendo as models do nosso aplicativo de porteiros

A ideia de representar as entidades do sistema em classes é interessante pois nos permite programar características e comportamentos específicos de cada entidade. Assim como escrevemos a classe `Usuario` como subclasse de `models.Model` para representar e descrever nossos usuários, vamos escrever a classe `Porteiro` que será a representação de nossos porteiros. 

Em nosso documento de requisitos é possível verificar que os porteiros devem possuir os seguintes atributos:

* Usuário para acesso ao sistema
* Nome completo
* CPF
* Telefone
* Data de nascimento

{% hint style="success" %}
Como nosso usuário é composto de um `e-mail` e um `tipo`, e o porteiro está vinculado a um usuário, obrigatoriamente o porteiro terá um e-mail
{% endhint %}

O primeiro passo será criar a classe `Porteiro` no arquivo `porteiros/models.py.` Como já sabemos os atributos que um porteiro deve ter e já usamos o campo do tipo `CharField` anteriormente, começaremos trabalhando nos atributos que são deste tipo \(nome completo, cpf e telefone\):

```python
from django.db import models

class Porteiro(models.Model):
    nome_completo = models.CharField(
        verbose_name="Nome completo",
        max_length=194
    )

    cpf = models.CharField(
        verbose_name="CPF",
        max_length=11,
    )

    telefone = models.CharField(
        verbose_name="Telefone de contato",
        max_length=11,
    )
```

### Conhecendo o campo CharField

### Conhecendo o campo OneToOneField

### Conhecendo o campo UUIDField

### Conhecendo o campo DateField



## Registrando nossa aplicação no Admin do Django

## Aplicando as alterações em nosso banco de dados








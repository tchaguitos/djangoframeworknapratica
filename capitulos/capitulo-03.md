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

Nossos três primeiros campos receberão apenas os argumentos `verbose_name` e `max_length`. O primeiro para dizer por qual nome devemos chamar o campo e o segundo para dizer o tamanho máximo permitido.

Apesar de CPF e telefone serem representados por números, possuem características e particularidades que fazem com que a gente trabalhe com eles como se fossem texto. Além disso, o telefone está com tamanho 11 pois vamos trabalhar com o DDD + 9 dígitos.

### Conhecendo o campo DateField

Sabemos também que, por exigência do setor de RH, é necessário informar a data de nascimento do porteiro para realização do cadastro. Sendo assim, vamos adicionar mais um campo ao nosso modelo, agora com nome de `data_nascimento` e o tipo data \(`DateField`\).

Os campos do tipo data representam datas - obviamente, mas é interessante a gente prestar atenção ao fato de que esses campos são representados por instâncias do tipo `datetime.date` que é como as datas são tratadas no Python e podem receber dois argumentos que ainda não conhecemos: `auto_now` e `auto_now_add`.

O argumento `auto_now` diz para o Django que é necessário atualizar o valor sempre que nosso objeto for salvo. Se nós definirmos ele como `True` \(verdadeiro\), o valor de `data_nascimento` será atualizado para um valor atual sempre que atualizarmos as informações de um porteiro. No caso do argumento `auto_now_add`, ele diz para o Django que é necessário inserir a data atual como valor no momento da criação e, feito isso, o valor não é atualizado para casos de atualização das informações.

Como nosso objetivo é informar uma data que represente a data de nascimento do porteiro, vamos dizer ao Django que não é necessário preencher o campo automaticamente apenas setando os valores dos argumentos como `False`. 

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
    
    data_nascimento = models.DateField(
        verbose_name="Data de nascimento",
        auto_now_add=False,
        auto_now=False
    )
```

### Conhecendo o campo OneToOneField

### Conhecendo o campo UUIDField



## Registrando nossa aplicação no Admin do Django

## Aplicando as alterações em nosso banco de dados








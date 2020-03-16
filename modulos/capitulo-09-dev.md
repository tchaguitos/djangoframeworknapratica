# Capítulo 09 - dev

## Criando função para autorizar entrada de visitante

Nos capítulos anteriores, nos dedicamos à criação das funcionalidades para registro e visualização de informações de visitantes. Aprendemos um pouco sobre como requisições funcionam, aprendemos a utilizar os formulários do Django, adaptamos os templates e aprendemos um pouco mais como eles funcionam, conhecemos o **django-widget-tweaks**, o Django **messages** e o `get_object_or_404` e ainda criamos métodos personalizados para nosso modelo de visitantes. Olha só quanta coisa conseguimos absorver somente nesses últimos capítulos!

Com essas funcionalidades criadas, precisamos seguir o roteiro que criamos com base nas necessidades do cliente, de modo que, agora, trabalharemos na funcionalidade que autoriza a entrada do visitante no condomínio. Quando um visitante chega na portaria e se identifica, o mesmo deve aguardar que o colaborador responsável entre em contato com um morador da casa a ser visitada e autorize que este visitante adentre ao condomínio.

Ao receber a informação de que o visitante pode adentrar ao condomínio, o porteiro deverá registrar na dashboard qual o nome do morador que autorizou a entrada deste visitante. Para fins de controle, também é necessário que o sistema registre o horário em que essa autorização ocorreu. Sendo assim, no momento da autorização do visitante, devemos registrar o horário e o nome do morador responsável pela autorização.

## Criando um status diferente para cada estágio da visita

Antes de seguir, precisamos analisar o cenário e extrair algumas informações com base nos fluxos e eventos que ocorrem, afim de melhorar a experiência de utilização da dashboard. Existem três cenários para o visitante: quando ele chega na portaria e está aguardando autorização \(status 1\), quando ele está dentro do condomínio realizando a visita \(status 2\) e quando ele vai embora e finaliza a visita \(status 3\).

Definir e tornar esses status explícitos é interessante pois com eles conseguimos dizer quantos usuários estão aguardando autorização, quantos estão dentro do condomínio e quantos já finalizaram a visita.

Vamos voltar ao nosso arquivo `models.py` do aplicativos **visitantes** e adicionar o atributo `status` ao modelo de Visitante. Ele será do tipo `CharField`, mas com a diferença que receberá uma lista pré determinada de opções disponíveis para escolha. Essa lista deverá guardar as opções disponíveis e devemos definir sempre o valor que será salvo em nosso banco de dados e o valor que será exibido para o usuário final. Antes do atributo, vamos criar a variável `STATUS_VISITANTE`, que ficará da seguinte forma:

```python
class Visitante(models.Model):

    STATUS_VISITANTE = [
        ("AGUARDANDO", "Aguardando autorização"),
        ("EM_VISITA", "Em visita"),
        ("FINALIZADO", "Visita finalizada"),
    ]
    
    # código abaixo omitido
```

{% hint style="info" %}
A lista `STATUS_VISITANTE` segue o que foi dito anteriormente e define os status `Aguardando autorização`, `Em visita` e `Finalizado`. O primeiro valor, em letras maiúsculas, será guardado no banco de dados e o segundo é o texto que será exibido para o usuário final. Sendo assim, quando o visitante receber o status `AGUARDANDO`, o texto **Aguardando autorização** será exibido.
{% endhint %}

Com isso, agora vamos adicionar o atributo `status` ao nosso modelo. Além dos argumentos `verbose_name` e `max_length` que já conhecemos, também vamos passar `choices` e `default`. O primeiro é a lista que criamos com as opções disponíveis e o segundo é o valor padrão a ser definido quando uma instância do modelo for criada. Nosso código ficará assim:

```python
class Visitante(models.Model):

    STATUS_VISITANTE = [
        ("AGUARDANDO", "Aguardando autorização"),
        ("EM_VISITA", "Em visita"),
        ("FINALIZADO", "Visita finalizada"),
    ]
    
    status = models.CharField(
        verbose_name="Status",
        max_length=22,
        choices=STATUS_VISITANTE,
        default="AGUARDANDO",
    )
    
    # código abaixo omitido
```

### Criando o arquivo de migrações

Como vimos anteriormente, sempre que mudamos a estrutura do nosso modelo, precisamos criar um arquivo que registra essas alterações em formato de migração para que seja possível efetuar as alterações no banco de dados posteriormente. Mais uma vez, utilizaremos o comando `makemigrations`:

```python
(env)$ python manage.py makemigrations visitantes
```

### Efetuando as alterações no banco de dados

Nada diferente do que já vimos, vamos agora utiliza o comando `migrate`:

```python
(env)$ python manage.py migrate visitantes
```

## Criando formulário para atualizar atributos específicos do visitante



## Alterando template para exibir modal com formulário

## Alterando formulário HTML para se adequar às nossas necessidades

* Alterando método do formulário HTML

## Atualizando os campos horario\_autorizacao e status diretamente

### Conhecendo o datetime do Python




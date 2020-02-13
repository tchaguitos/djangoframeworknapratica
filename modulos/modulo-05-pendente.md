# Módulo 05 - dev

## Criando o aplicativo para gerenciar visitantes

No último capítulo nós configuramos o Django para trabalhar com templates HTML e a pasta templates na raíz do projeto e ainda aprendemos como definir informações em uma view e exibí-las e no template.

Como já definimos os usuários do sistema, os porteiros e fizemos as configurações dos templates que serão a base para construção da dashboard para controle de visitantes, podemos partir agora para definição do modelo de visitante.

Antes de mais nada, como já sabemos, devemos isolar as responsabilidades e, por isso, vamos criar um aplicativo para administrar toda a parte referente aos nossos visitantes. Vamos criar um novo aplicativo com nome de **visitantes** utilizando o `manage.py`:

```text
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

Feito isso, vamos começar os trabalhos no arquivos `models.py` para definirmos as informações necessárias para nosso modelo de visitante.

## Escrevendo as models do nosso aplicativo de visitantes

Conforme falamos, nossa camada _model_ ****\(ou camada de modelo\) é nossa fonte segura de dados e também onde definimos o formato das informações que serão disponibilizadas para outras camadas da aplicação. Sendo assim, temos que definir quais informações desejamos guardar dos nossos visitantes.

A partir do documento de requisitos, podemos concluir que é necessário guardar uma série de informações a respeito de quem deseja adentrar ao condomínio para realizar visita a moradores, além da autorização de um morador que esteja na casa no momento da visita. O procedimento faz parte de normas do condomínio para fins de fiscalização, controle e segurança dos moradores. As informações contidas nas normas do condomínio e que devem ser fornecidas pelo visitante são:

1. Nome completo
2. CPF
3. Data de nascimento
4. Número da casa a ser visitada
5. Placa do veículo utilizado na visita, se houver
6. Horário da chegada
7. Horário da saída
8. Nome do morador responsável por autorizar a entrada
9. Horário de autorização de entrada
10. Porteiro responsável por registrar visitante

Inicialmente, vamos nos concentrar nas informações de 1 a 7 para que possamos avaliar por partes o modelo de visitante. Vamos escrever primeiro os atributos nome completo, CPF, data de nascimento, número da casa e placa do veículo, pois são todos tipos de dados que já conhecemos. Nossa classe `Visitante` ficará assim:

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

    numero_casa = models.CharField(
        verbose_name="Número da casa a ser visitada",
        max_length=3,
    )

    placa_veiculo = models.CharField(
        verbose_name="Placa do veículo",
        max_length=10,
        blank=True,
        null=True,
    )
```

### Conhecendo o campo DateTimeField





Além destas informações, é necessário que seja guardado também o nome do morador que autorizou a entrada do visitante no condomínio. Sendo assim, teremos mais dois atributos:

* Nome do responsável por autorizar a entrada do visitante
* Horário de autorização de entrada

### Conhecendo o campo ForeignKey

## Aplicando as alterações nas models no banco de dados

## Registrando nossa aplicação no Admin do Django

## Registrando um visitante utilizando o Django Admin

## Listando visitantes na página inicial da dashboard

### Buscando registros de visitantes no banco de dados

* Falar sobre Queryset API

### Listando registros de visitantes no HTML


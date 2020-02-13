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

### Conhecendo o campo ForeignKey

### Conhecendo o campo DateTimeField

## Aplicando as alterações nas models no banco de dados

## Registrando nossa aplicação no Admin do Django

## Registrando um visitante utilizando o Django Admin

## Listando visitantes na página inicial da dashboard

### Buscando registros de visitantes no banco de dados

* Falar sobre Queryset API

### Listando registros de visitantes no HTML


# Nova estrutura do curso

Módulo 1

* [x] Conhecendo o instrutor e sabendo mais sobre o curso
* [x] A linguagem de programação Python
* [x] O que é Django Framework?
* [x] Instalando e configurando o ambiente para iniciar o projeto

## Módulo 2

* [x] Iniciando seu primeiro projeto Django
* [x] Entendendo a estrutura do projeto
* [x] Criando nosso primeiro aplicativo Django
* [x] Criando nosso "Hello World"
* [x] Escrevendo as models
* [x] Criando um super usuário
* [x] Conhecendo o Django Admin
* [x] Boas práticas e alterações necessárias nas configurações

## Módulo 3

* [x] Criando nosso aplicativo para gerenciar porteiros
* [x] Escrevendo as models do nosso aplicativo de porteiros
  * [x] Conhecendo o campo DateField
  * [ ] Conhecendo o campo UUIDField
  * [ ] Conhecendo o campo OneToOneField
* [ ] Aplicando as alterações nas models no banco de dados
* [ ] Registrando nossa aplicação no Admin do Django

## Módulo 4

* [ ] Configurando a aplicação para trabalhar com arquivos estáticos e pasta templates
* [ ] Explicando as adaptações realizadas no template para funcionar no Django
  * [ ] Alterar o caminho para arquivos estáticos
* [ ] Atualizar função de home para usar o template
* [ ] Passando variáveis para o template

## Módulo 5

* [ ] Criando nosso aplicativo para gerenciar visitantes
* [ ] Escrevendo as models do nosso aplicativo de visitantes
  * [ ] Conhecendo o campo ForeignKey
  * [ ] Conhecendo o campo DateTimeField
* [ ] Aplicando as alterações nas models no banco de dados
* [ ] Registrando nossa aplicação no Admin do Django
* [ ] Listando visitantes na página inicial da dashboard
  * [ ] Buscando registros de visitantes no banco de dados
  * [ ] Listando registros de visitantes no HTML

## Módulo 6

* [ ] Criando tela para registro de visitante
* [ ] Adaptando o template para trabalhar com a template engine do Django
  * [ ] Criando o template "base.html"
  * [ ] Adaptando template "registrar\_visitante.html" para trabalhar com a engine do Django
* [ ] Criando view para registrar visitante
* [ ] Trabalhando com formulários no Django
  * [ ] Preparando view para receber requisição do tipo POST \(falar sobre "request", validações dos formulários do Django \(modelForm\), falar sobre redirect e csrf\_token\)
  * [ ] Criando intimidade com os formulários \(falar sobre atributo fields e alterar para se adequar às nossas necessidades\)
* [ ] Conhecendo um pouco mais dos formulários \(tratar problema de valor nulo para campo "autorizado\_por"\)
  * [ ] Falar sobre relacionamento entre FKs

## Módulo 7

* [ ] Melhorando a exibição do nosso formulário
* [ ] Estilizando nosso formulário com django-widget-tweaks
  * [ ] Como instalar \(inserindo nas configurações\)
  * [ ] Importando no template \(tags, etc\)
  * [ ] Utilizando o add\_class
* [ ] Exibindo uma mensagem para o usuário ao cadastrar novo visitante \([https://github.com/tchaguitos/controle-visitantes/commit/c414e3068e2fffd9272c4e8fee306d287a21219b](https://github.com/tchaguitos/controle-visitantes/commit/c414e3068e2fffd9272c4e8fee306d287a21219b)\)
  * [ ] Conhecendo o Django messages
  * [ ] Adicionando uma mensagem
  * [ ] Alterando o template para exibir as mensagens
* [ ] Utilizando o Django para renderizar nossas URLs

## Módulo 8

* [ ] Criando tela para exibir informações de visitante
  * [ ] Criando view
    * [ ] Recebendo token em view
    * [ ] Utilizando o get\_model\_or\_404
  * [ ] Criando template
  * [ ] Criando URL para acessar informações de visitante
    * [ ] Recebendo variáveis através da URL no Django
* [ ] Aprendendo um pouco mais sobre URLs no Django
* [ ] Modelos do Django e seus métodos \([https://docs.djangoproject.com/en/2.2/ref/models/instances/\#django.db.models.Model.get\_FOO\_display](https://docs.djangoproject.com/en/2.2/ref/models/instances/#django.db.models.Model.get_FOO_display)\)
  * [ ] get\_status\_display\(\)
* [ ] Criando métodos personalizados


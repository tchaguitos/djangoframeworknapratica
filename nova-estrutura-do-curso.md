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

## Módulo 6

* [ ] Criando tela para registro de visitante
* [ ] Adaptando o template para trabalhar com a template engine do Django
  * [ ] Criando o template "base.html"
  * [ ] Criar template "index.html" utilizando a tag "block" e "extends
  * [ ] Adaptando template "registrar\_visitante.html" para trabalhar com a engine do Django
* [ ] Criando view para registrar visitante
* [ ] Trabalhando com formulários no Django
  * [ ] Criando intimidade com os formulários \(falar sobre atributo fields e alterar para se adequar às nossas necessidades\)
* [ ] Melhorando a exibição do nosso formulário
* [ ] Estilizando nosso formulário com django-widget-tweaks
  * [ ] Como instalar \(inserindo nas configurações\)
  * [ ] Importando no template \(tags, etc\)
  * [ ] Utilizando o add\_class
* [ ] Preparando nossa view para receber uma requisição do tipo POST
  * [ ] Falar sobre o objeto "request"
  * [ ] Falar sobre as validações dos formulários do Django \(modelForm\)
  * [ ] Falar sobre redirect
  * [ ] Falar sobre csrf\_token
* [ ] Exibindo uma mensagem para o usuário ao cadastrar novo visitante
  * [ ] Conhecendo o Django messages
  * [ ] Adicionando uma mensagem
  * [ ] Alterando o template para exibir as mensagens
    * [ ] Adicionar o HTML do alert na página "base.html" e explicar que o Django messages é um middleware global, etc
* [ ] Adicionando função para botão "cancelar" de formulário de cadastro de visitante


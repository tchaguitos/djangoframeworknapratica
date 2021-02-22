# Tópicos da apostila

## Capítulo 01

* Conhecendo o instrutor e sabendo mais sobre o curso
  * Sobre o instrutor
  * Sobre o curso
  * O que iremos construir?
* A linguagem de programação Python
* O que é Django Framework?
* Instalando e configurando o ambiente para iniciar o projeto
  * Python 3.8
  * Virtualenv
  * Pip
  * Django framework

## Capítulo 02

* Iniciando seu primeiro projeto Django
  * Um pouco mais sobre o django-admin
* Entendendo a estrutura do projeto
* Criando nosso primeiro aplicativo Django
* Escrevendo nosso "Hello World"

## Capítulo 03

* Escrevendo as models
  * Escrevendo a classe modelo
  * Escrevendo um manager personalizado
  * Alterando o modelo padrão nas configurações
  * Criando as tabelas do nosso banco de dados
* Criando um super usuário
* Conhecendo o Django Admin
* Alterações necessárias nas configurações



## Capítulo 04

* Criando aplicativo para gerenciar porteiros
* Escrevendo as models do nosso aplicativo de porteiros
  * Conhecendo o campo DateField
  * Conhecendo o campo OneToOneField
* Registrando nossa aplicação no Admin do Django
* Aplicando as alterações em nosso banco de dados
* Criando porteiro através do Admin do Django

## Capítulo 05

* Configurando a aplicação para trabalhar com arquivos estáticos e templates HTML
  * Criando a pasta templates em nosso projeto
  * Criando a pasta static em nosso projeto
* Criando views que renderizam templates
  * Conhecendo a função render
* Entendendo as adaptações necessárias no template
  * Conhecendo a tag static
  * Alterando o caminho dos arquivos estáticos
* Exibindo variáveis no template
  * Definindo nosso dicionário de contexto
  * Exibindo as informações nos templates

## Capítulo 06

* Criando o aplicativo para gerenciar visitantes
* Escrevendo as models do nosso aplicativo de visitantes
  * Conhecendo o campo DateTimeField
  * Conhecendo o campo ForeignKey
* Registrando nossa aplicação no Admin do Django
* Aplicando as alterações em nosso banco de dados
* Adicionando visitante utilizando o Django Admin
* Listando visitantes na página inicial da dashboard
  * Buscando registros de visitantes no banco de dados
  * Listando registros de visitantes no template HTML

## Capítulo 07

* Criando tela para registro de novo visitante
* Criando view para registrar visitante
  * Criando URL para mapear view
* Adaptando nossos templates para trabalhar com a template engine do Django
  * Criando o template base
  * Adaptando template index
  * Adaptando template base
  * Adaptando template registrar\_visitante

## Capítulo 08

* Trabalhando com formulários no Django
* Criando formulário para registro de visitante
* Renderizando nosso formulário automaticamente
* Melhorando a exibição do nosso formulário
* Estilizando nosso formulário com django-widget-tweaks
  * Como instalar
  * Importando no template
  * Utilizando o render\_field

## Capítulo 09

* Preparando view para receber requisição do tipo POST
  * Conhecendo o objeto request
* Conhecendo um pouco mais dos formulários
  * Tratando problema com atributo nulo
* Exibindo uma mensagem para o usuário ao cadastrar novo visitante
  * Conhecendo o Django messages
  * Alterando o template para exigir as mensagens
* Tratando possíveis erros em nosso formulário
* Deixando nossas mensagens de erro mais claras

## Capítulo 10

* Criando tela para exibir informações de visitante
* Criando a view
  * Conhecendo o atalho get\_model\_or\_404
* Criando URL para acessar informações de visitante
* Criando template para exibir informações de visitante
* Criando métodos personalizados para exibir informações do Visitante
  * Criando método para exibir horário de saída
  * Criando métodos para exibir horário de autorização de entrada e morador responsável por autorizar a entrada
  * Criando método para exibir placa do veículo utilizado na visita
* Utilizando métodos personalizados no template
* Utilizando o Django para renderizar nossas URLs

## Capítulo 11

* Criando funcionalidade para autorização de entrada de visitante
* Criando um status diferente para cada estágio da visita
  * Criando o arquivo de migrações
  * Efetuando as alterações no banco de dados
* Criando formulário para atualizar atributos específicos do visitante
* Alterando view para autorizar entrada de visitante
* Alterando template para exibir modal com formulário
* Atualizando os campos horario\_autorizacao e status diretamente
  * Atualizando o status
  * Conhecendo o timezone do Django

## Capítulo 12

* Criando função para finalizar visita
* Criando URL
* Alterando template para exibir botão e modal para finalizar visita
* Prevenindo erros e operações desnecessárias
  * Exibição condicional de botões para autorizar entrada e finalizar visita
* Bloqueando o acesso à URL por métodos diferentes do POST

## Capítulo 13

* Implementando melhorias em nossos templates
  * Exibindo botão com função de "voltar" e "cancelar" em páginas de informações e registro de visitante
  * Melhorando a exibição do CPF do visitante
  * Criando método para exibir status do visitante
* Implementando melhorias na estrutura do nosso projeto

## Capítulo 14

* Criando aplicativos para administrar informações da dashboard
* Migrando view "index" para aplicativo dashboard
* Conhecendo o método filter das querysets
  * Filtrando nossos visitantes por status
  * Contando os resultados de uma queryset

## Capítulo 15

* Aprendendo a filtrar nossos visitantes por data
* Conhecendo o field lookups da Queryset API
* Filtrando apenas os registros do mês atual
  * Utilizando o timezone para descobrir o mês atual
* Ordenando nossa lista de visitantes por horário de chegada

## Capítulo 16

* Bloqueando o acesso para usuários não autenticados nas nossas views
  * Conhecendo o decorator login\_required
* Alterando a URL padrão para login e redirecionamento após login
* Utilizando o sistema de autenticação do Django para nos fornecer a view de login
* Criando o template de login
  * Renderizando formulário de login
* Adicionando mensagem de erro em formulário de login
* Criando URL para logout
* Criando template de logout
* Inserindo link para logout em dashboard




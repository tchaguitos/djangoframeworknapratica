# Capítulo 12 - dev

## Criando aplicativos para administrar informações da dashboard

No capítulo anterior nós começamos a implementar algumas melhorias visando uma melhor experiência de utilização da nossa dashboard e ainda atuamos de forma a melhorar a estrutura do projeto para facilitar futuras manutenções. Seguindo nesse mesmo caminho, vamos criar um aplicativo chamado **dashboard** para administrar melhor as informações relacionadas principalmente à página inicial da dashboard. É importante também a gente lembrar que os aplicativos de um projeto Django devem dividir responsabilidades e cada um deles ter um único objetivo. 

Apesar da gente ter finalizado as principais funcionalidades da dashboard, ainda precisamos buscar alguns números para que sejam mostrados na página inicial. Se você observar o template, vai perceber que existem elementos que nos sugerem que devemos exibir o número de visitantes de cada status e quantos visitantes tivemos no mês atual. Queremos fazer algo desse tipo:

![](../.gitbook/assets/screenshot_2020-04-08_12-21-52.png)

Todas essas informações podem ser tiradas a partir da queryset que busca todos os visitantes no banco de dados. Em breve vamos aprender como podemos fazer isso e tornar os dados na nossa dashboard dinâmicos.

Mas antes de tudo, vamos criar o aplicativo utilizando o `manage.py`:

```bash
(env)$ python manage.py startapp dashboard
```

E depois vamos adicionar o novo aplicativo ao arquivo de configurações, o `settings.py`:

```python
INSTALLED_APPS += [
    "usuarios",
    "porteiros",
    "visitantes",
    "dashboard",
]
```

## Migrando view "index" para aplicativo dashboard

Agora que nós temos um aplicativo para gerenciar as informações da nossa dashboard, vamos migrar a função de view index para o aplicativo dashboard. Para isso, vamos copiar

## Contando o número total de visitantes para exibir na home da dashboard

* Conhecendo o método count\(\)
* Exibindo número de visitantes no template

## Conhecendo o método filter das querysets

* Filtrando nossos visitantes por nome e CPF

## Filtrando e contando nossos visitantes por status

* Filtrando e contando visitantes em visita
* Filtrando e contando visitantes aguardando autorização
* Filtrando e contando visitas finalizadas


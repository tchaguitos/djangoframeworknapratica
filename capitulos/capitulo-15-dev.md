# Capítulo 15 - dev

## Paginando listagem de visitantes

Nossa dashboard está praticamente finalizada e pronta para ser entrega ao cliente. Criamos uma dashboard que lista os visitantes recentes e apresenta alguns dados de forma resumida na página inicial, registra novos visitantes, visualiza informações de visitantes e autoriza e finaliza as visitas. Além disso, criamos também toda a parte de login e logout para que os usuários possam se autenticar para visualizar as URLs que agora não mais permitem acessos não autenticados.

No decorrer do curso, aprendemos sobre diversas ferramentas que o Django framework nos dá e vimos como utilizá-las em um projeto do mundo real. A última ferramenta a ser conhecida e melhoria a ser implementada, será a paginação dos resultados dos visitantes recentes. Por agora, com poucos visitantes registrados, não temos problemas, mas pense se o banco de dados tiver 5000 registros de visitantes. Não seria interessante a gente buscar todo esse volume de dados de uma vez e muito menos exibí-los na template, certo?

Pensando nisso, o Django possui a classe Paginator, que é vai nos ajudar a paginar os resultados do nosso banco de dados. 





## Conhecendo a classe Paginator

## Alterando template para exibir resultados paginados

* 10 em 10 itens

## Adicionando links no template para navegar nos resultados


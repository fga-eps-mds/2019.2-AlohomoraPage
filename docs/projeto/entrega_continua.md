# Entrega Contínua

## Introdução

Esse documento tem como intuito explicitar os estágios do processo de integração contínua do backend e frontend do projeto, mapear seus respectivos ambientes de Homologação e Produção, enumerar e explicar as tecnologias utilizadas para a criação dos pipelines, e listar os endereços de usuário do Telegram das versões utilizáveis dos Bots.

## Ferramentas

* Travis-ci: Executar os pipelines de Integração Contínua;
* Pytest: Framework para testes unitários no Backend;
* Pylint: Ferramenta para checar erros de código e forçar padronização de estilo;
* Codeclimate: Monitoramento de qualidade do código.
* Docker/Docker-compose: Conteinerização dos serviços para agilizar desenvolvimento;
* Coveralls: Checagem de Cobertura de testes do projeto;
* Heroku: Plataforma em nuvem para deploy dos ambientes de Homologação e Produção.

## Pipeline Backend
[Histórico do Pipeline](https://travis-ci.org/Alohomora-team/AlohomoraAPI/requests)

### Estágios

Os estágios da pipeline do Backend são divididos em dois grupos de tarefas que rodam em paralelo com o intuito de agilizar a sua execução. Esses dois grupos são:

#### Testes de Qualidade de Código
* Lint: Procura por erros no código e desvios do padrão de estilo;
* Test: Checa que todos os testes unitários estão passando;
* Coveralls: Gera a métrica de cobertura de código.

#### Push da Imagem e Deploy
* Push da Imagem: Realiza-se o build da imagem da aplicação e é feito o push para o dockerhub com o intuito de agilizar o desenvolvimento;
* Deploy: Realiza-se o build e o Deploy da imagem da aplicação para o ambiente no Heroku.

### Ambientes

Ao surgirem alterações nas branches devel e master, realiza-se o deploy da aplicação automaticamente para o ambiente de Homologação e Produção respectivamente. Estes abmientes são:

* Homologação: https://alohomora-hmg.herokuapp.com/
* Produção: https://alohomora-prod.herokuapp.com/

## Pipeline Frontend(Bot)
[Histórico do Pipeline](https://travis-ci.org/Alohomora-team/2019.2-AlohomoraBot/requests)

### Estágios

* Lint: Procura por erros no código e desvios do padrão de estilo;
* Deploy: Realiza-se o build do bot e, após isso, realiza-se o Deploy da aplicação para o ambiente no Heroku.

### Ambientes

Ao surgirem alterações nas branches devel e master, realiza-se o deploy do bot automaticamente para o ambiente de Homologação e Produção respectivamente. Estes abmientes são:

* Homologação: https://alohomora-bot-hmg.herokuapp.com/
* Produção: https://alohomora-bot.herokuapp.com/

## Entrega Contínua

Após ser realizado um deploy, os bots de Homologação e Produção automaticamente ficam disponiveis para interação nos sequintes usuários do Telegram:

* Homologação: @AlohoHmgBot
* Produção: @AlohoBot
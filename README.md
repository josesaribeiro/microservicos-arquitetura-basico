# Básico de Micro Serviços

Repositório de anotações referentes ao curso básico sobre a Arquitetura de Micro Serviços.

### Links

- [Microservices Architecture](https://app.pluralsight.com/library/courses/microservices-architecture/table-of-contents)

- [Artigos Sobre](https://microservices.io/)

## O que é um Serviço?
Um Serviço é basicamente um Software que irá prover funcionalidades à outros Software.

![](https://i.imgur.com/YhgVf6v.png)

### Benefícios
Micro Serviços não é uma bala de prata, mas possuí uma série de benefícios.

Ao desvincular o código do tradicional Monolito, estaremos criando sistemas fáceis de serem distribuídos, promoveremos a desacoplação e permitiremos que várias equipes de desenvolvedores efetuem seu papel, de forma hábil e independente.

- Ciclos de desenvolvimento mais curtos
- Deploy rápido e confiável
- Habilita updates com frequência
- Desacopla o sistema em pequenas partes
- Segurança
- Uptime
- Resolução rápida de issues
- Altamente escaláveis e melhor performance
- Melhor "conhecimento e sentimento de propriedade"
- Tecnologia certa
- Habilita equipes distribuídas

## Microservices Design Principles

Lembre-se, arquitetura baseada em Microserviços é um Design complexo de Software e deve ser efetuada com cuidado. Como o sistema estará quebrado em partes desacopladas, diversas tarefas que seriam simples em outros ambientes podem se tornar mais complexa aqui.

Por tanto, é importante se manter fiéis aos principios do Microserviço para garantir a qualidade do sistema sendo desenvolvido.

### High Coesion
Deve seguir o **Single Responsibility** do conceito **SOLID**. Ou seja, deve ter apenas uma Responsabilidade, um Serviço deve estar responsável por fazer apenas uma coisa, e fazer ela muito bem feita.

Deve alterar por apenas uma razão.

**Dicas:**
- Identifique um foco único
- Evite "é tipo a mesma coisa"
- Não seja preguiçoso!
- Não tenha medo de criar muitos serviços (apesar que, não será do dia para noite que você fará mil microserviços)

### Autonomous
Desacoplação. Uma alteração em um Microserviço, não deve prejudicar nenhum outro. Ele deve ser trocável com outro micro-serviço que implemente as mesmas interfaces. Deve ter seu deploy de forma independente. 

**Dicas:**
- Sua API de comunicação deve ser independete de tecnologias específicas (Ex: REST, Event Queue..)
- Crie contratos entre os serviços (interfaces fixas e comum. modelos compartilhados)
- Evite múltiplas comunicação entre os mesmos serviços para finalizar uma tarefa
- Evite o compartilhamento de banco de dados

### Business Domain Centric
Cada Serviço deve representar um `Bounded Context` do DDD. É uma delimitação de escopo de uma regra de negócio da empresa.

**Dicas:**
- Aplique os conceitos de DDD (linguagem ubíqua, separação de domínio etc..)
- Revise constantamente as separações
- Corrija separações incorretas (merge ou split)
- Interfaces abertas ao público
- Se necessário, crie micro-serviços responsáveis apenas por separações técnicas

### Resilience
Os Serviços devem ser resilientes, isto é, devem **abraçar falhas**. A falha pode ser uma falha de conexão com outro serviço, banco de dados, api de terceiros, etc..

O Serviço deve estar preparado para responder de acordo caso algum problema aconteça, registrar logs, salvar o estado, manter a integridade dos dados, reiniciar etc..

**Dicas:**
- Projete seu serviço para falhas conhecidas
- Degrade a funcionalidade em caso de falha (use uma funcionalidade não tão boa, ou mesmo algo padrão)
- Projete o sistema para falhar rápido (fail fast)
- Use timeouts para a comunicação entre serviços
- Definida um tempo padrão de tamanho para timeout
- Monitore os timeouts

### Observable
Ao utlizar microserviços precisamos ter ferramentas para monitorar o **Status** do servidor, **Logs** e **Erros**. Pois caso alguma "ponta" do sistema dê problema, poderemos demorar à ter um Feedback, caso não haja nenhum tipo de aviso.

**Dicas (Monitoramento):**
- Monitoramento real time
- Monitore os servidores (CPU, Memoria, Disco..)
- Exponha as métricas dos serviços (Response time, Timeout, Erros..)
- Ferramentas de análise do negócio (trends, vendas, produtos, checkouts, comparações..)

**Dicas (Logging):**
- Registre informações ao iniciar o sistema e ao finalizar
- Timeouts, exceções, erros..
- Estruture os Logs por Nível
- Rastreie a transação, para acompnhar o passo a passo para chegar no erro 

### Automation
Ferramentas de redução de alguns testes (configuração de ambiente, regressão manual, tempo de teste de integração)

Feedback de integração de Continuous Integration.

Ferramentas para providenciar um deploy rapido e automatizado. 

É importante que essas ferramentas automatizadas existam pois fazer manualmente o  deploy e testes de integração em uma arquitetura baseada em Microserviços, se torna algo muito demorado, cansativo e não confiável.

**Dicas :**
- Ferramentas de Versionamento
- Lint automático
- Testes unitário e de integração obrigatório
- Aplicar ferramentas de CI (Continuous Integration) e CD (Continuous Deployment)

## Tecnologias para Microservices

### Synchronous Communication
- RPC (Remote Procedure Call)
- HTTP (REST ou não)

Problema: Ambas partes tem que estarem disponíveis

### Asynchronous Communication

Mitiga a necessidade de disponibilidade entre o cliente e o serviço. Desacopla-os.

`Subscriber` e `Publisher` estarão desacoplados.

- RabbitMQ
- Microsoft message queuing (MSMQ)
- ATOM (HTTP to Propagate Events)

**Desafios:**
- Complicado
- Necessiade do Message Broker
- Visiblidade da Transação
- Gerenciando a fila

Em sistemas reais, deverá ser utilizando ambos comunicação Síncrona e Assíncrona.

![](https://i.imgur.com/IRDiu4j.png)

### Virtualization

Ao invés de rodar o micro-serviço na máquina principal, é possível virtualizá-lo para rodar numa VM. Dessa forma, mantemos os micro-serviços isolados e podemos alternar facilmente de tecnologia.

- Platform as a Service (PAAS)
	- Azure
	- AWS
	- Sua própria Cloud (vSphere)
	
	
### Container

Uma forma diferente de Virtualização, mas mais leve do que uma VM tradicional. Capaz de isola os serviços de forma separada, tendo um serviço por container. Usa menos recursos e é mais rápido que uma VM tradicional.

- Docker
- Rocker
- Glassware

### Registration and Discovery

Alguns hosts como Azure, possuem soluções pronta, para permitir que o seu serviço se Registre e Desregistre.

O ideal, é que caso alguma falha esteja ocorrendo, o serviço se Desresgistre antes de cair, para que a aplicação pare de tentar acessar aquele serviço, ou seja redirecionada para um fallback.

### Monitoring Tech

**Centralizado:**
- Nagios
- PRTG
- Load Balancers
- New Relic

**Desejado:**
- Metricas entre servidores
- Configuração mínima e automática
- Libs para enviar métricas
- Suporte à teste de transações
- Alerta
- Monitoramento de Rede
- Monitoramento Real Time

### Logging Tech
A stack de Log difere do monitormento no seguinte, o Monitoramento deve salvar **Números**, a Stack de **Log** deve registrar informações.

**Logs Centralizado:**
- Elastic Log
- Log Stash
- Splunk
- Kibana
- Graphite

**Client Logging Libraries:**
- Serilog

**Desejado:**
- Log estruturado
- Log entre servidores
- Configuração miníma ou automática
- Correlação entre transações
- Template
- Ferramentl Central

### Scaling

**Possibilidades:**
- Criar múltiplas instâncias de um serviço
- Adicionar recursos de forma dinâmica
- Múltiplos Hosts + Load Balances

AWS e Azure possuí opções de Auto Scalling que escalam sua aplicação de acordo com a demanda.

### Caching

Deve ser simples de configurar e gerenciar. Devemos tomar cuidado, não podemos integrar informações incorretas ao cliente.

### Automation Tools

**CI:**
- Team Foundation Server
- TeamCity

## Brownfield Projects

Em projetos **Monoliticos** que já existem há um tempo e possuem toneladas de código legado, não é interessante tentar migrar de uma vez para microserviços.

O correto, é analisar o sistema, identificar as separações de domínio, levantar a estrutura ideal para microserviços (ferramentas de CI, CD, Log, Monitoramento, Message Broker etc..), e ir efetuando a migração de forma incremental.

Deve-se avaliar o que precisar ter prioridade na separação, e sempre efetuar testes unitários para garantir que a aplicação não está quebrando. 

**Migração do DB:**
- Evite bancos compartilhados
- Refatore o banco de dados em múltiplos DB
- Divida o banco de dados de acordo com os serviços
- Chamadas de API para capturar dados de relecionamento
- Garanta integridade por meio da API
- Tabelas estáticas ou compartilhada provavelmente deverão ter seu microserviço separado

**Transações:** 

Como em microserviços o sistema estará dividido em múltiplos banco de dados, transacionar as operações se torna um procedimento complexo em N frentes (observar, solução, rollback etc..).

Uma opção, é usar algo como um `Transaction Manager` que deverá gerenciar nossos microserviços e gerar um commit de duas etapas. Nesse modelo, cada serviço deverá implementar uma forma de `Fazer`, `Desfazer` e `Comitar` sua operação, caso um falhe, o transaction manager deverá fazer com que todas as pontas efetuem um rollback.

**Relatórios:**

Com o banco de dados dividido, fica difícil gerar Relatórios pois perdemos a capacidade de efetuar Joins e relacionar os dados.

Possibilidades:
- Utilizar API's para retornar os dados de cada microserviço (pode ser lerdo)
- Consolidar os dados em um banco centralizado (usar um cronjob para normatizar)

## Greenfield Projects

Comece com um Design Monolítico e vá efetuando a divisão e separação, conforme a identificação dos `Bounded Context`.

Modulos se tornarão serviços. Bibliotecas compartilhadas também serão promovidas à serviços.

Refine e refatore a arquitetura constantamente. Revise os princípios da Arquitetura de Microserviço a cada estágio de desenvolvimento.

Priorize o MVP (Minimal Viable Product) e as necessaides do Cliente.

## Premissa

Deve-se aceitar os gastos iniciais: Tempo maior de desenvolvimento. Custo de ferramentas e treinamentos de novas habilidades. Lidar com transações distribuídas e relatórios.

Haverá custos adicionais destinados à teste de Latencia, Performance e Resiliencia.

Lembre que Microserviços se comunicam pela Rede, dessa forma, a Infraestrutura de Rede também deve ser constantemente monitorada.

**Quebra de cultura!**

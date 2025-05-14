# Enterprise-Challenge---Sprint-1
Enterprise Challenge - Sprint 1 - Reply
# FIAP - Inteligência artificial e data science

<p align="center">
<a href= "https://www.fiap.com.br/"><img src="assets/logo-fiap.png" alt="FIAP - Faculdade de Informática e Admnistração Paulista" border="0" width=40% height=40%></a>
</p>

<br>

# Nome do projeto
Cap 3 - Colheita de Dados e Insights - dados valiosos e maduros - Enterprise Challenge - Sprint 1

## Nome do grupo
21

## 👨‍🎓 Integrantes: 
- <a href="https://www.linkedin.com/company/inova-fusca">Guilherme Campos Hermanowski </a>
- <a href="https://www.linkedin.com/company/inova-fusca">Gabriel Viel </a>
- <a href="https://www.linkedin.com/company/inova-fusca"> Matheus Alboredo Soares</a> 
- <a href="https://www.linkedin.com/company/inova-fusca">Jonathan Willian Luft </a>

## 👩‍🏫 Professores:
### Tutor(a) 
- <a href="https://www.linkedin.com/company/inova-fusca">Leonardo Ruiz Orabona</a>
### Coordenador(a)
- <a href="https://www.linkedin.com/company/inova-fusca">ANDRÉ GODOI CHIOVATO</a>


## 📜 Justificativa do problema e descrição da solução proposta

<br>

Em cenários de produção onde há um grande número de maquinário atuando, é rotineiro que diferentes tipos de erros e falhas que acabem por gerar prejuízos e atrapalhar no andamento da produção aconteçam.
Mas e se esses prejuízos e paradas na produção pudessem ser previstos, e assim, antecipadamente evitados, dessa otimizando os processos de melhorando o fluxo de trabalho da empresa? É a partir dessa visão de negócio que surge nosso projeto. 
<br>
Com foco no monitoramento e previsão de falhas em equipamentos de produção, utilizamos de sensores de temperatura, vibração, umidade e volume de produção, somado a uma arquitetura baseada em serviços AWS, para detecção de falhas antes que elas ocorram, permitindo que alertas sejam gerados e o erro evitado antes de sua incidência.


## 🔧 Componentes
- Com variado uso de ferramentas AWS e lingaugens voltadas a análise de dados, o programa seria composto da seguinte forma:

1.	Sensores (ESP32);
2.	AWS IoT Core;
3.	Armazenamento de Dados;
4.	Amazon S3;
5.	Amazon RDS;
6.	AWS Lake Formation;
7.	AWS Lambda;
8.	Amazon SageMaker;
9.	AWS Step Functions;
10.	Amazon CloudWatch;
11.	SNS.
    

## 🔧 Funcionamento

O projeto se inicia com a recolhimento de dados pelos 4 sensores, onde cada um deles (controlados por um ESP32) coletam as informações de temperatura, umidade, vibração e volume de produção do ambiente. Após isso, as informações coletadas são enviadas para o AWS IoT Core via MQTT.
Com Amazon S3 e Amazon RDS os dados são armazenados no banco de dados, onde o AWS Lake Formation organiza esses dados através da criação de um Data Lake para facilitar o gerenciamento e a análise. 
Isto feito e todos os dados devidamente armazenados, o serviço do AWS Lambda é acionado e verifica a existência de novos dados, para que, caso existam, seja iniciado o treinamento do modelo de machine learning via Amazon SageMaker.
O gerenciamento de todo esse processo de treinamento se dá através do AWS Step Functions, enquanto CloudWatch e SNS enviam alertas caso algo dê errado ou o treinamento seja concluído.



## 📁 Estrutura de pastas

Sem pastas


## 🗃 Histórico de lançamentos

* 0.1.0 - 18/04/2024
    *

## 📋 Licença

<img style="height:22px!important;margin-left:3px;vertical-align:text-bottom;" src="https://mirrors.creativecommons.org/presskit/icons/cc.svg?ref=chooser-v1"><img style="height:22px!important;margin-left:3px;vertical-align:text-bottom;" src="https://mirrors.creativecommons.org/presskit/icons/by.svg?ref=chooser-v1"><p xmlns:cc="http://creativecommons.org/ns#" xmlns:dct="http://purl.org/dc/terms/"><a property="dct:title" rel="cc:attributionURL" href="https://github.com/agodoi/template">MODELO GIT FIAP</a> por <a rel="cc:attributionURL dct:creator" property="cc:attributionName" href="https://fiap.com.br">Fiap</a> está licenciado sobre <a href="http://creativecommons.org/licenses/by/4.0/?ref=chooser-v1" target="_blank" rel="license noopener noreferrer" style="display:inline-block;">Attribution 4.0 International</a>.</p>

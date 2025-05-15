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
</p>
Com foco no monitoramento e previsão de falhas em equipamentos de produção, utilizamos de sensores de temperatura, vibração, umidade e volume de produção, somado a uma arquitetura baseada em serviços AWS, para detecção de falhas antes que elas ocorram, permitindo que alertas sejam gerados e o erro evitado antes de sua incidência.


## 🔧 Componentes
**Definição das tecnologias que serão utilizadas (linguagens de programação, bibliotecas de IA, serviços de nuvem, banco de dados etc.):**
</p>
***AWS IoT Core:***
•	Definição: Permite conectar dispositivos físicos (como ESP32) à nuvem de forma segura, confiável e escalável.
•	Linguagem: MQTT, HTTP, TLS (via certificados).
•	Propósito: Receber os dados dos sensores do ambiente físico (temperatura, vibração, entre outras coletas) e encaminhá-los para o RDS.
•	Funcionamento: O dispositivo publica mensagens para um tópico MQTT, o IoT Core aplica regras de roteamento para enviar esses dados diretamente para RDS.

***Amazon RDS:***
</p>
•	Definição: Banco de dados relacional, sem a necessidade de um EC2 e diminuindo atribuições como manutenção, configuração e atualizações de sistema Operacional, Redes ou Backup por exemplo.
•	Linguagem: SQL
•	Proposito: Armazenar os dados bruto do sensor, para garantir dados originais e também quaisquer logs adicionais pela equipe de IA (resultados de treinamentos por exemplo) ou estrutura relacional nova para atender escalabilidade da arquitetura de banco.

***Armazenamento S3 + Lake:***
</p>
•	Definição: Armazenamento (S3) em nuvem e governança e controle de acesso sobre o armazenament (Lake Formation)
•	Integração: Através de replicação de dados do RDS e Lambda
•	Propósito: Ter um repositório sem impactar em ambiente produtivo (RDS) e também possibilitando uma futura fonte de dados para construção de Dashboards, além de servir de fonte de dados para a IA
•	Funcionamento: Assim que realizado um UPLOAD mapeado no S3, é diparado um gatilho para o Lambda acessar e dar inicio as etapas referentes aos dados para a IA.

***Amazon Lambda:***
</p>
•	Definição: Permitir executar código em resposta a eventos
•	Linguagem: Python
•	Propósito: Realizar o pré processamento deles disparados pelo S3 e realizar a carga para o Amazon SageMaker, além também de servir para possível carga de dados no banco produtivo, referente a algum log a ser registrado no RDS.
•	Funcionamento: Disparado pelo S3 ou para carga de dados no RDS.(Em resumo uma ferramenta da AWS para integração de fluxos)

***Amazon SageMaker:***
</p>
•	Definição:  Plataforma de machine learning gerenciada para criar, treinar, implantar e monitorar modelos de aprendizado de máquina.
•	Linguagem: TensorFlow, R, Pandas e Numpy
•	Integração: É acionado após o Lambda receber e fazer o pré processamento desses dados do S3.
•	Propósito: Processar os dados recebidos e realizar inferência com base nos modelos treinados, como detectar os padrões dos logs recebidos do sensor ESP32 e poder gerar uma análise preditiva.
•	Funcionamento: Recebe os dados do Lambda, executa a inferência com o modelo implantado e retorna a resposta, podendo registrar algum resultado no RDS (através do Lambda), ou disparando notificações para os usuários responsáveis sobre o equipamento monitorado em especifico daquele sensor.

***AWS Step Functions:***
</p>
•	Definição: Coordenar a execução sequencial e condicional de vários serviços, para fluxos mais longos ou lógica mais complexa
•	Linguagem: Podemos criar o Fluxo visualmente pelo console da AWS ou por exemplo chamar uma função Lambda escrita em Python.
•	Propósito: Organizar fluxos complexos em etapas visuais com controle de erro, espera, decisão e paralelismo.

***Amazon CloudWatch:***
</p>
•	Definição: Monitoramento e observação de métricas, logs e alarmes de recursos da AWS.
•	Integração: Coleta logs e métricas do Lambda, monitora uso do SageMaker, e pode disparar  SNS ou outra função Lambda com base em condições.
•	Propósito: Acompanhar o comportamento do sistema e criar automações baseadas em falhas ou condições predefinidas.
•	Funcionamento: Analisa as métricas ou logs, acompanha os processos e disparar alertas via SNS ou outras funções de recursos.

***Amazon SNS (Simple Notification Service):***
</p>
•	Definição: Envio de alertas e notificações por e-mail, SMS ou outras aplicações
•	Propósito : Integrado com o Lambda ou diretamente com CloudWatch. Pode ser acionado com base nos resultados da IA, pela observação do CloudWatch em resposta a um evento, no nosso caso o acionamento em decorrência da identificação de problemas pela análise preditiva da IA e notificar  o responsável técnico pelo tipo de equipamento coletado pelo sensor que acusou o possível problema antes de ocorrer a parada em produção.
•	Funcionamento: Se a inferência do SageMaker indicar uma condição anormal, o Lambda ou Step Function publica uma mensagem no SNS que é entregue ao responsável via email, sms ou por alguma aplicação.


## 📁 Arquitetura e Pipeline

![Captura de tela 2025-05-14 205646](https://github.com/user-attachments/assets/b8ca6629-2493-4338-a6b6-65c57a923d91)



## 🔧 Funcionamento

O projeto se inicia com a recolhimento de dados pelos 4 sensores, onde cada um deles (controlados por um ESP32) coletam as informações de temperatura, umidade, vibração e volume de produção do ambiente. Após isso, as informações coletadas são enviadas para o AWS IoT Core via MQTT.
Com Amazon S3 e Amazon RDS os dados são armazenados no banco de dados, onde o AWS Lake Formation organiza esses dados através da criação de um Data Lake para facilitar o gerenciamento e a análise. 
Isto feito e todos os dados devidamente armazenados, o serviço do AWS Lambda é acionado e verifica a existência de novos dados, para que, caso existam, seja iniciado o treinamento do modelo de machine learning via Amazon SageMaker.
O gerenciamento de todo esse processo de treinamento se dá através do AWS Step Functions, enquanto CloudWatch e SNS enviam alertas caso algo dê errado ou o treinamento seja concluído.

## 👨‍🎓 Divisão de responsabilidades:
- Arquitetura (Pipeline e estrutura de features na AWS) : <a href="https://www.linkedin.com/company/inova-fusca">Gabriel Viel </a>
- Coleta de dados: <a href="https://www.linkedin.com/company/inova-fusca">Jonathan Willian Luft </a> e <a href="https://www.linkedin.com/company/inova-fusca">Guilherme  Campos Hermanowski </a>
- Banco de Dados: <a href="https://www.linkedin.com/company/inova-fusca">Gabriel Viel </a>
- Treinamento de IA: <a href="https://www.linkedin.com/company/inova-fusca"> Matheus Alboredo Soares</a> 
- Integração de Features: <a href="https://www.linkedin.com/company/inova-fusca">Gabriel Viel </a>, <a href="https://www.linkedin.com/company/inova-fusca"> Matheus Alboredo Soares</a>, <a href="https://www.linkedin.com/company/inova-fusca">Jonathan Willian Luft </a> e <a href="https://www.linkedin.com/company/inova-fusca">Guilherme  Campos Hermanowski </a>



## 📋 Licença

<img style="height:22px!important;margin-left:3px;vertical-align:text-bottom;" src="https://mirrors.creativecommons.org/presskit/icons/cc.svg?ref=chooser-v1"><img style="height:22px!important;margin-left:3px;vertical-align:text-bottom;" src="https://mirrors.creativecommons.org/presskit/icons/by.svg?ref=chooser-v1"><p xmlns:cc="http://creativecommons.org/ns#" xmlns:dct="http://purl.org/dc/terms/"><a property="dct:title" rel="cc:attributionURL" href="https://github.com/agodoi/template">MODELO GIT FIAP</a> por <a rel="cc:attributionURL dct:creator" property="cc:attributionName" href="https://fiap.com.br">Fiap</a> está licenciado sobre <a href="http://creativecommons.org/licenses/by/4.0/?ref=chooser-v1" target="_blank" rel="license noopener noreferrer" style="display:inline-block;">Attribution 4.0 International</a>.</p>

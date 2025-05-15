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
```
**AWS IoT Core:**

  •	***Definição:*** Permite conectar dispositivos físicos (como ESP32) à nuvem de forma segura, confiável e escalável.<br>
  •	***Linguagem:*** MQTT, HTTP, TLS (via certificados).<br>
  •	***Propósito:*** Receber os dados dos sensores do ambiente físico (temperatura, vibração, entre outras coletas) e encaminhá-los para o RDS.<br>
  •	***Funcionamento:*** O dispositivo publica mensagens para um tópico MQTT, o IoT Core aplica regras de roteamento para enviar esses dados diretamente para RDS.<br>

**Amazon RDS:**

  •	***Definição:*** Banco de dados relacional, sem a necessidade de um EC2 e diminuindo atribuições como manutenção, configuração e atualizações de sistema Operacional, Redes ou Backup por exemplo.<br>
  •	***Linguagem:*** SQL<br>
  •	***Proposito:*** Armazenar os dados bruto do sensor, para garantir dados originais e também quaisquer logs adicionais pela equipe de IA (resultados de treinamentos por exemplo) ou estrutura relacional nova para atender escalabilidade da arquitetura de banco.<br>

**Armazenamento S3 + Lake:**

  •	***Definição:*** Armazenamento (S3) em nuvem e governança e controle de acesso sobre o armazenament (Lake Formation).<br>
  •	***Integração:*** Através de replicação de dados do RDS e Lambda.<br>
  •	***Propósito:*** Ter um repositório sem impactar em ambiente produtivo (RDS) e também possibilitando uma futura fonte de dados para construção de Dashboards, além de servir de fonte de dados para a IA.<br>
  •	***Funcionamento:*** Assim que realizado um UPLOAD mapeado no S3, é diparado um gatilho para o Lambda acessar e dar inicio as etapas referentes aos dados para a IA.<br>

**Amazon Lambda:**

  •	***Definição:*** Permitir executar código em resposta a eventos.<br>
  •	***Linguagem:*** Python.<br>
  •	***Propósito:*** Realizar o pré processamento deles disparados pelo S3 e realizar a carga para o Amazon SageMaker, além também de servir para possível carga de dados no banco produtivo, referente a algum log a ser registrado no RDS.<br>
  •	***Funcionamento:*** Disparado pelo S3 ou para carga de dados no RDS.(Em resumo uma ferramenta da AWS para integração de fluxos).<br>

**Amazon SageMaker:**

  •	***Definição:***  Plataforma de machine learning gerenciada para criar, treinar, implantar e monitorar modelos de aprendizado de máquina.<br>
  •	***Linguagem:*** TensorFlow, R, Pandas e Numpy.<br>
  •	***Integração:*** É acionado após o Lambda receber e fazer o pré processamento desses dados do S3.<br>
  •	***Propósito:*** Processar os dados recebidos e realizar inferência com base nos modelos treinados, como detectar os padrões dos logs recebidos do sensor ESP32 e poder gerar uma análise preditiva.<br>
  •	***Funcionamento:*** Recebe os dados do Lambda, executa a inferência com o modelo implantado e retorna a resposta, podendo registrar algum resultado no RDS (através do Lambda), ou disparando notificações para os usuários responsáveis sobre o equipamento monitorado em especifico daquele sensor.<br>

**AWS Step Functions:**

  •	***Definição:*** Coordenar a execução sequencial e condicional de vários serviços, para fluxos mais longos ou lógica mais complexa.<br>
  •	***Linguagem:*** Podemos criar o Fluxo visualmente pelo console da AWS ou por exemplo chamar uma função Lambda escrita em Python.<br>
  •	***Propósito:*** Organizar fluxos complexos em etapas visuais com controle de erro, espera, decisão e paralelismo.<br>

**Amazon CloudWatch:**

  •	***Definição:*** Monitoramento e observação de métricas, logs e alarmes de recursos da AWS.<br>
  •	***Integração:*** Coleta logs e métricas do Lambda, monitora uso do SageMaker, e pode disparar  SNS ou outra função Lambda com base em condições.<br>
  •	***Propósito:*** Acompanhar o comportamento do sistema e criar automações baseadas em falhas ou condições predefinidas.<br>
  •	***Funcionamento:*** Analisa as métricas ou logs, acompanha os processos e disparar alertas via SNS ou outras funções de recursos.<br>

**Amazon SNS (Simple Notification Service):**

  •	***Definição:*** Envio de alertas e notificações por e-mail, SMS ou outras aplicações.<br>
  •	***Propósito :*** Integrado com o Lambda ou diretamente com CloudWatch. Pode ser acionado com base nos resultados da IA, pela observação do CloudWatch em resposta a um evento, no nosso caso o acionamento em decorrência da identificação de problemas pela análise preditiva da IA e notificar  o responsável técnico pelo tipo de equipamento coletado pelo sensor que acusou o possível problema antes de ocorrer a parada em produção.<br>
  •	***Funcionamento:*** Se a inferência do SageMaker indicar uma condição anormal, o Lambda ou Step Function publica uma mensagem no SNS que é entregue ao responsável via email, sms ou por alguma aplicação.<br>
```

## 📁 Arquitetura e Pipeline

![Captura de tela 2025-05-14 205646](https://github.com/user-attachments/assets/b8ca6629-2493-4338-a6b6-65c57a923d91)



## 🔧 Funcionamento

O sistema utiliza uma arquitetura de monitoramento inteligente na AWS, com integração entre dispositivos físicos, banco de dados, machine learning e notificações automáticas. O ESP32 envia dados de sensores (como temperatura e vibração) via MQTT para o AWS IoT Core, com comunicação segura usando TLS e autenticação por certificados. Esses dados são roteados diretamente para o Amazon RDS, um banco de dados relacional gerenciado, onde são armazenados utilizando SQL.

Os dados do RDS são replicados para o Amazon S3 com controle de acesso via Lake Formation, permitindo análises futuras e integrando com funções AWS Lambda escritas em Python. Nessas funções Lambda são utilizadas bibliotecas como pandas para tratamento de dados, e boto3 para interação com os serviços da AWS. Quando um novo dado chega ao S3, um gatilho aciona o Lambda para pré-processar e encaminhar os dados ao Amazon SageMaker.

O SageMaker, também operando em Python, utiliza bibliotecas como TensorFlow, scikit-learn, pandas e numpy para treinar e executar modelos de machine learning. Ele realiza inferência sobre os dados do sensor e detecta padrões que possam indicar falhas iminentes. Os resultados podem ser salvos no RDS ou repassados para outras funções Lambda.

A coordenação de todos esses fluxos é feita com AWS Step Functions, onde os fluxos são definidos visualmente ou com código em JSON, orquestrando chamadas de funções Lambda e decisões baseadas em resultados.

Para monitoramento, o Amazon CloudWatch coleta logs e métricas das funções Lambda e do SageMaker, permitindo a criação de alarmes automatizados. Caso uma anomalia seja detectada, CloudWatch pode acionar o Amazon SNS, que envia notificações por e-mail ou SMS ao responsável técnico, garantindo ação preventiva antes de falhas críticas.

## 👨‍🎓 Divisão de responsabilidades:
- Arquitetura (Pipeline e estrutura de features na AWS) : <a href="https://www.linkedin.com/company/inova-fusca">Gabriel Viel </a>
- Coleta de dados: <a href="https://www.linkedin.com/company/inova-fusca">Jonathan Willian Luft </a> e <a href="https://www.linkedin.com/company/inova-fusca">Guilherme  Campos Hermanowski </a>
- Banco de Dados: <a href="https://www.linkedin.com/company/inova-fusca">Gabriel Viel </a>
- Treinamento de IA: <a href="https://www.linkedin.com/company/inova-fusca"> Matheus Alboredo Soares</a> 
- Integração de Features: <a href="https://www.linkedin.com/company/inova-fusca">Gabriel Viel </a>, <a href="https://www.linkedin.com/company/inova-fusca"> Matheus Alboredo Soares</a>, <a href="https://www.linkedin.com/company/inova-fusca">Jonathan Willian Luft </a> e <a href="https://www.linkedin.com/company/inova-fusca">Guilherme  Campos Hermanowski </a>



## 📋 Licença

<img style="height:22px!important;margin-left:3px;vertical-align:text-bottom;" src="https://mirrors.creativecommons.org/presskit/icons/cc.svg?ref=chooser-v1"><img style="height:22px!important;margin-left:3px;vertical-align:text-bottom;" src="https://mirrors.creativecommons.org/presskit/icons/by.svg?ref=chooser-v1"><p xmlns:cc="http://creativecommons.org/ns#" xmlns:dct="http://purl.org/dc/terms/"><a property="dct:title" rel="cc:attributionURL" href="https://github.com/agodoi/template">MODELO GIT FIAP</a> por <a rel="cc:attributionURL dct:creator" property="cc:attributionName" href="https://fiap.com.br">Fiap</a> está licenciado sobre <a href="http://creativecommons.org/licenses/by/4.0/?ref=chooser-v1" target="_blank" rel="license noopener noreferrer" style="display:inline-block;">Attribution 4.0 International</a>.</p>

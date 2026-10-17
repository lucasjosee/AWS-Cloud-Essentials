### Conceitos básicos
Cada um é um subconjunto do anterior:
- **Inteligência Artificial (IA)**: sistemas que fazem tarefas que parecem exigir inteligência humana (reconhecer imagens, entender texto, conversar).
- **Machine Learning (ML)**: parte da IA que **aprende padrões a partir de dados** para fazer previsões, sem ser programada regra por regra.
- **Deep Learning**: parte do ML que usa **redes neurais** com muitas camadas.
- **IA generativa**: deep learning que **cria conteúdo novo** (texto, imagem, código, áudio). Usa **modelos de base** (*foundation models*), modelos gigantes pré-treinados que se adaptam a várias tarefas. Os **LLMs** (modelos de linguagem) são um tipo de modelo de base.

### As 3 camadas de IA/ML na AWS
| Camada | O que é | Para quem |
| --- | --- | --- |
| **1. Serviços de IA prontos** | APIs já treinadas que fazem **uma tarefa específica** (Rekognition, Polly, Comprehend...). | Quem **não tem experiência em ML**. Só chama a API. |
| **2. Serviços de ML** | **Amazon SageMaker**: plataforma para **criar, treinar e implantar seus próprios modelos**. | Cientistas de dados e desenvolvedores que precisam de um **modelo personalizado**. |
| **3. Frameworks e infraestrutura** | TensorFlow, PyTorch, Deep Learning AMIs, instâncias EC2 com GPU. | Especialistas que querem **controle total**. |

### Na prova
- “Sem experiência em ML”, “sem treinar modelo”, “adicionar IA ao app rapidamente” → **serviço de IA pronto** (camada 1)
- “Treinar um modelo personalizado com os próprios dados” → **SageMaker**
- **Pegadinha:** se a tarefa já tem um serviço pronto (ex.: transcrever áudio), a resposta é o serviço pronto, **não** o SageMaker.

### Serviços de IA prontos
#### Visão (imagens, vídeos e documentos)
- **Amazon Rekognition**: analisa **imagens e vídeos**. Detecta objetos, pessoas, cenas, **rostos**, reconhecimento facial, celebridades e **conteúdo impróprio**.
- **Amazon Textract**: **extrai texto e dados de documentos digitalizados** (formulários, tabelas, notas fiscais, PDFs). Vai além do OCR comum, pois entende campos e tabelas.

#### Linguagem (texto e voz)
- **Amazon Comprehend**: **PLN** (processamento de linguagem natural). Analisa texto e encontra **sentimento** (positivo/negativo), entidades, palavras-chave e idioma. Também detecta **dados pessoais (PII)** em texto.
- **Amazon Translate**: **tradução** de texto entre idiomas.
- **Amazon Transcribe**: **fala → texto** (transcrição de áudio, legendas, ligações de call center).
- **Amazon Polly**: **texto → fala** (voz realista, ex.: ler artigos em voz alta).

#### Conversação, busca e personalização
- **Amazon Lex**: cria **chatbots** e interfaces de conversa por voz ou texto (a mesma tecnologia da Alexa).
- **Amazon Kendra**: **busca inteligente** em documentos da empresa, entendendo perguntas em linguagem natural.
- **Amazon Personalize**: **recomendações personalizadas** em tempo real (“quem comprou isso também comprou...”), a mesma tecnologia da Amazon.com.

> O **Kendra entrou em modo de manutenção** em 30/06/2026 e **não aceita clientes novos desde 30/07/2026**. Continua na prova, então saiba para que serve.

### Na prova
- “Identificar rostos/objetos em fotos ou vídeos”, “moderar imagens impróprias” → **Rekognition**
- “Extrair dados de formulários, notas fiscais, documentos digitalizados” → **Textract**
- “Analisar **sentimento** de avaliações/comentários de clientes” → **Comprehend**
- “Traduzir o site/app para outros idiomas” → **Translate**
- “Transcrever ligações”, “gerar legendas” → **Transcribe**
- “Converter texto em voz”, “ler em voz alta” → **Polly**
- “Chatbot”, “assistente de voz para atendimento” → **Lex**
- “Buscar respostas em documentos internos da empresa” → **Kendra**
- “Recomendar produtos” → **Personalize**
- **Pegadinhas:**
  - **Rekognition** = o que **aparece** na imagem. **Textract** = o **texto/dados** do documento.
  - **Transcribe** = **fala→texto**. **Polly** = **texto→fala**. A prova inverte os dois.
  - **Comprehend** **entende** o texto. **Translate** só **traduz**.
  - Um fluxo comum combina vários: ligação → **Transcribe** (vira texto) → **Comprehend** (sentimento) → **Translate** (outro idioma).

### Amazon SageMaker
Serviço **totalmente gerenciado** para **criar, treinar e implantar** modelos de ML em escala, sem cuidar da infraestrutura. Hoje se chama **Amazon SageMaker AI**.
- Cobre o ciclo inteiro: preparar dados → treinar → ajustar → implantar → monitorar.
- **SageMaker JumpStart**: catálogo de **modelos pré-treinados** (inclusive de IA generativa) para começar rápido.

#### Outros serviços de ML que aparecem no curso
| Serviço | Para quê | Situação |
| --- | --- | --- |
| **Amazon Forecast** | Previsão de séries temporais (vendas, estoque) | Não aceita clientes novos desde 2024 |
| **Amazon Fraud Detector** | Detectar fraudes online | Não aceita clientes novos desde 07/11/2025 |
| **Amazon Lookout for Metrics** | Detectar anomalias em métricas | Encerrado em 2025 |
| **AWS DeepLens** | Câmera com deep learning (aprendizado) | Encerrado em 2024 |
| **AWS Deep Learning AMIs** | AMIs do EC2 já com frameworks de ML instalados | Ativo |

> Como estão sendo descontinuados, basta reconhecer o nome e o uso.

### IA generativa na AWS
- **Amazon Bedrock**: acesso a **modelos de base de vários fornecedores** (Anthropic, Meta, Mistral, Amazon...) **por API**, **sem servidor** (*serverless*) e sem gerenciar infraestrutura. Permite personalizar os modelos com os dados da empresa, e **esses dados não são usados para treinar os modelos de base**.
- **SageMaker JumpStart**: modelos de base prontos para **implantar e ajustar dentro do SageMaker** (mais controle que o Bedrock).
- **Amazon Q**: **assistente de IA generativa** pronto para uso.
  - **Amazon Q Business**: responde perguntas e faz tarefas usando os **dados da empresa** (documentos, sistemas internos).
  - **Amazon Q Developer**: ajuda desenvolvedores a **escrever código** e a trabalhar com a AWS.

### Na prova
- “Criar app de IA generativa usando **vários modelos de base**, sem gerenciar infraestrutura” → **Bedrock**
- “Implantar e ajustar um modelo de base com controle total do ML” → **SageMaker JumpStart**
- “Assistente que responde perguntas sobre os dados internos da empresa” → **Amazon Q Business**
- “Assistente que gera código / ajuda devs na AWS” → **Amazon Q Developer**

---

### Análise de dados (Data Analytics)
**ETL (Extract, Transform, Load)**: **extrair** os dados das fontes, **transformar** (limpar, padronizar) e **carregar** num destino central para análise.

**Data lake × Data warehouse:**
- **Data lake**: guarda **dados brutos de todo tipo** (estruturados ou não) em grande volume e baixo custo. Na AWS: **S3**.
- **Data warehouse**: dados **estruturados e já organizados** para consultas analíticas e relatórios. Na AWS: **Redshift** (ver [[Banco de Dados]]).

**Etapas de um pipeline de dados** (decore os serviços por etapa):

| Etapa | Serviços |
| --- | --- |
| **1. Coletar / ingerir** | **Kinesis**, **Data Firehose**, MSK |
| **2. Armazenar** | **S3** (data lake), **Redshift** (data warehouse) |
| **3. Catalogar e preparar (ETL)** | **Glue** |
| **4. Analisar / processar** | **Athena**, **EMR**, **Redshift**, **OpenSearch** |
| **5. Visualizar** | **QuickSight** |

#### Coleta de dados em tempo real (streaming)
- **Amazon Kinesis Data Streams**: coleta e processa **dados em tempo real** (cliques em sites, logs, sensores de IoT, transações).
- **Amazon Data Firehose** (antigo *Kinesis Data Firehose*): **entrega** dados de streaming de forma automática para **S3, Redshift, OpenSearch** etc. Pode transformar os dados com Lambda no caminho. Totalmente gerenciado, sem configurar nada de capacidade.
- **Amazon MSK**: **Apache Kafka** gerenciado. Mesmo uso do Kinesis, para quem já usa Kafka.

#### Preparação dos dados
- **AWS Glue**: serviço de **ETL sem servidor** (*serverless*). Descobre, limpa, transforma e move os dados. O **Glue Data Catalog** guarda os metadados (esquema das tabelas), usados pelo Athena e pelo EMR.

#### Análise
- **Amazon Athena**: faz **consultas SQL direto nos arquivos do S3**, **sem servidor** e sem carregar os dados em um banco. Paga **por quantidade de dados lidos** na consulta.
- **Amazon Redshift**: **data warehouse** para análises pesadas e recorrentes em grandes volumes de dados estruturados (OLAP).
- **Amazon EMR**: **clusters gerenciados de big data** com **Hadoop, Spark**, Hive etc. Para processamento massivo.
- **Amazon OpenSearch Service**: **busca e análise de logs** em tempo quase real (antigo Elasticsearch). Ex.: pesquisar dentro de logs da aplicação, busca em sites.

#### Visualização
- **Amazon QuickSight**: **BI (business intelligence)**. Cria **dashboards** e gráficos interativos a partir dos dados. Em outubro/2025 virou o **Amazon Quick Sight**, parte do pacote **Amazon Quick Suite**, mas a prova ainda usa o nome QuickSight.

#### Outros
- **AWS Data Exchange**: **marketplace de dados de terceiros**. Para comprar/assinar conjuntos de dados externos.

### Na prova
- “Dados em **tempo real**/streaming”, “cliques”, “IoT”, “telemetria” → **Kinesis**
- “Carregar dados de streaming no S3/Redshift sem gerenciar nada” → **Data Firehose**
- “Kafka gerenciado” → **MSK**
- “ETL”, “preparar/limpar/catalogar dados”, “serverless” → **Glue**
- “Consultar arquivos no S3 com **SQL**, sem servidor”, “consultas ad hoc” → **Athena**
- “Data warehouse”, “análise de dados históricos”, “relatórios em grande escala” → **Redshift**
- “**Hadoop** / **Spark**”, “big data em clusters” → **EMR**
- “Pesquisar e analisar **logs**”, “busca em texto” → **OpenSearch**
- “**Dashboards**”, “**BI**”, “visualizar dados” → **QuickSight**
- “Comprar dados de terceiros” → **Data Exchange**
- **Pegadinhas:**
  - **Athena × Redshift**: Athena = consultas **pontuais** direto no S3, sem infraestrutura. Redshift = data warehouse para análises **frequentes e pesadas**.
  - **Glue × EMR**: Glue = ETL **sem servidor**. EMR = **clusters** de Hadoop/Spark que você configura.
  - **Kinesis × SQS**: Kinesis = **fluxo contínuo** de dados para analisar em tempo real. SQS = **fila** de mensagens para desacoplar apps.
  - **OpenSearch × Kendra**: OpenSearch = busca/análise de **logs e dados**. Kendra = busca inteligente em **documentos** com linguagem natural (IA).
  - **QuickSight × CloudWatch**: QuickSight = dashboards de **dados de negócio**. CloudWatch = métricas de **infraestrutura** da AWS.

### Resumo: qual serviço escolher?
| Cenário da questão | Resposta |
| --- | --- |
| Analisar imagens/vídeos, reconhecer rostos | **Rekognition** |
| Extrair texto e tabelas de documentos | **Textract** |
| Sentimento / análise de texto | **Comprehend** |
| Traduzir texto | **Translate** |
| Áudio → texto | **Transcribe** |
| Texto → áudio | **Polly** |
| Chatbot | **Lex** |
| Busca inteligente em documentos internos | **Kendra** |
| Recomendações personalizadas | **Personalize** |
| Treinar e implantar modelo próprio de ML | **SageMaker** |
| App de IA generativa com modelos de base via API | **Bedrock** |
| Assistente de IA para empresa ou desenvolvedores | **Amazon Q** |
| Streaming de dados em tempo real | **Kinesis** |
| ETL sem servidor | **Glue** |
| SQL direto no S3 | **Athena** |
| Data warehouse | **Redshift** |
| Big data com Hadoop/Spark | **EMR** |
| Busca e análise de logs | **OpenSearch** |
| Dashboards / BI | **QuickSight** |

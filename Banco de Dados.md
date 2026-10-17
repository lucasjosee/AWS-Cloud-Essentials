### Banco de dados no EC2 x banco gerenciado
Dá para instalar um banco de dados numa instância EC2, mas aí **você** cuida de tudo: patches do SO e do banco, backups, alta disponibilidade e escalabilidade. Só vale quando precisa de **controle total** ou de um mecanismo/versão que os serviços gerenciados não suportam.

Num banco **gerenciado** (RDS, Aurora, DynamoDB...) a AWS faz o trabalho pesado e você foca nos dados e nas consultas.

**Responsabilidade compartilhada no RDS:**
- **AWS**: hardware, instalação e **patches do SO e do banco**, backups automáticos, failover do Multi-AZ.
- **Cliente**: os **dados**, usuários e permissões do banco, **grupos de segurança**, ativar criptografia, escolher se quer Multi-AZ e réplicas.

### Bancos de Dados Relacionais
Dados em **tabelas** (linhas e colunas) com relacionamentos entre elas, consultados com **SQL** (joins). Esquema fixo.

#### Amazon RDS
O **RDS (Relational Database Service)** roda bancos de dados relacionais (SQL) sem que você precise cuidar do servidor. A AWS cuida de:

- instalar e aplicar atualizações no banco
- fazer **backups automáticos** e permitir restaurar para um momento específico (*point-in-time recovery*)
- **Multi-AZ**: manter uma réplica em espera em outra AZ, com failover automático → **alta disponibilidade**
- **réplicas de leitura**: cópias que atendem consultas e aliviam o banco principal → **desempenho de leitura**

**Mecanismos compatíveis**
	A solução Amazon RDS é compatível com diferentes mecanismos de banco de dados, inclusive Amazon Aurora, MySQL, PostgreSQL, Microsoft SQL Server, MariaDB e Oracle Database.

#### Amazon Aurora: o motor do RDS
O Aurora é um banco de dados relacional gerenciado **criado pela AWS** e projetado para ajudar a reduzir operações de E/S desnecessárias. Ele é compatível com MySQL e PostgreSQL, oferece alto desempenho e disponibilidade, além de se adaptar de forma automática às workloads. O Aurora replica dados em várias Zonas de Disponibilidade para maior durabilidade e tolerância a falhas, além de oferecer atributos de backups automatizados, criptografia em repouso e monitoramento contínuo.

- Até **5x mais rápido que o MySQL** e **3x mais rápido que o PostgreSQL** padrão.
- Guarda **6 cópias dos dados em 3 AZs**.
- Custa cerca de **1/10 de bancos comerciais** (Oracle, SQL Server).
- **Aurora Serverless**: a capacidade sobe e desce sozinha, para cargas intermitentes ou imprevisíveis.

### Na prova

- “Banco relacional **gerenciado**”, “sem gerenciar o servidor do banco” → **RDS**
- “Compatível com **MySQL e PostgreSQL**”, “**alto desempenho**”, “alta disponibilidade”, “criado pela AWS” → **Aurora**
- “Banco relacional com capacidade que se ajusta automaticamente” → **Aurora Serverless**
- “Aliviar consultas de leitura do banco principal” → **réplicas de leitura**
- “Failover automático para outra AZ” → **Multi-AZ**
- “Controle total do SO do banco” ou “mecanismo não suportado pelo RDS” → **banco no EC2**
- **Pegadinha:** Multi-AZ **não** melhora leitura (a réplica em espera não atende consultas). Réplica de leitura **não** é a resposta para alta disponibilidade.

### Banco de Dados não Relacionais
Também chamados de **NoSQL**. Não usam tabelas fixas: o esquema é **flexível** (cada item pode ter atributos diferentes). Ex.: chave-valor, documentos, grafos.

#### Amazon DynamoDB
O DynamoDB é um serviço de banco de dados NoSQL **totalmente gerenciado e sem servidor (serverless)** que fornece desempenho rápido e previsível para estruturas de dados de documentos e **chave-valor**.
- Latência de **milissegundos de um dígito** em qualquer escala
- Esquema flexível (sem joins)
- Ótimo para aplicações que exigem alta velocidade e scaling contínuo: carrinho de compras, sessões de usuários, placares de jogos, IoT

O DynamoDB aumenta ou diminui de forma automática o throughput de acordo com o uso real, o que garante um desempenho consistente sem intervenção manual.

O DynamoDB oferece disponibilidade de 99,99% e replica os dados automaticamente em **três AZs** da Região. Para ter cópias em **várias Regiões**, ativa-se o **Global Tables** (multi-região, multi-ativo, até 99,999% de disponibilidade).

- **DAX (DynamoDB Accelerator)**: cache em memória **só para o DynamoDB**, reduz a leitura de milissegundos para **microssegundos**.

### Na prova

- “NoSQL”, “chave-valor”, “**serverless**”, “milissegundos de um dígito”, “escala massiva” → **DynamoDB**
- “Mesmo banco DynamoDB em várias Regiões” → **DynamoDB Global Tables**
- “Acelerar leituras do DynamoDB para microssegundos” → **DAX**
- **Pegadinha:** a questão falar em **joins**, SQL ou relacionamentos complexos → **RDS/Aurora**, não DynamoDB.

### Bancos de dados com finalidade específica (*purpose-built*)
A ideia da AWS é usar **um banco certo para cada tipo de dado**, em vez de forçar tudo num relacional. Na prova basta reconhecer o nome e o uso:

| Serviço                | Tipo                                                | Palavra-chave na prova                                                                            |
| ---------------------- | --------------------------------------------------- | ------------------------------------------------------------------------------------------------- |
| **Amazon ElastiCache** | **Cache em memória** (Valkey, Redis OSS, Memcached) | “cache”, “dados acessados com frequência”, “reduzir carga do banco”, “latência de microssegundos” |
| **Amazon MemoryDB**    | Banco em memória **durável** compatível com Redis   | “banco principal em memória”, “ultrarrápido e sem perder dados”                                   |
| **Amazon DocumentDB**  | Banco de **documentos** (JSON), NoSQL               | “compatível com **MongoDB**”, catálogos, perfis de usuário                                        |
| **Amazon Neptune**     | Banco de **grafos**                                 | “redes sociais”, “recomendações”, “detecção de fraudes”, “dados altamente conectados”             |
| **Amazon Keyspaces**   | Colunas largas                                      | “compatível com **Apache Cassandra**”                                                             |
| **Amazon Timestream**  | **Séries temporais**                                | “dados de IoT ao longo do tempo”, métricas                                                        |
| **Amazon QLDB**        | **Ledger** (registro imutável)                      | “histórico de transações imutável e verificável”                                                  |

> O **QLDB foi descontinuado em 31/07/2025** (a AWS recomenda migrar para Aurora PostgreSQL), mas ainda pode aparecer em simulados antigos.

**Pegadinha:** o ElastiCache **não é onde os dados ficam guardados de verdade**. Ele fica na frente de um banco (RDS, DynamoDB) só para acelerar as leituras.

### Data warehouse: Amazon Redshift
- **Data warehouse** totalmente gerenciado, para **análise de grandes volumes de dados** (petabytes) e **BI (business intelligence)**.
- **OLAP** (análises grandes e complexas sobre dados históricos) × **OLTP** (muitas transações pequenas e rápidas do dia a dia, que é o RDS/Aurora/DynamoDB).
- Palavras-chave: “data warehouse”, “BI”, “analisar dados históricos”, “relatórios”.

**Pegadinha:** RDS = **transações** do sistema. Redshift = **análise**. A prova coloca os dois como alternativas.

### Migração de bancos de dados
#### AWS Database Migration Service (DMS)
Migra bancos para a AWS (ou entre bancos) com **tempo de inatividade mínimo**: o banco de origem **continua funcionando** durante a migração.
- **Homogênea**: mesmo mecanismo (ex.: MySQL → RDS MySQL) → só o DMS.
- **Heterogênea**: mecanismos diferentes (ex.: Oracle → Aurora) → precisa antes do **AWS Schema Conversion Tool (SCT)** para converter o esquema, e depois o DMS move os dados.
- Também serve para: desenvolvimento e testes com cópia dos dados de produção, consolidar vários bancos em um, replicação contínua.

**Pegadinha:** DMS **move os dados**. SCT **converte o esquema** quando o mecanismo muda.

### Resumo: qual banco escolher?
| Cenário da questão | Resposta |
| --- | --- |
| Relacional gerenciado (MySQL, PostgreSQL, Oracle, SQL Server, MariaDB) | **RDS** |
| Relacional de alto desempenho, compatível com MySQL/PostgreSQL | **Aurora** |
| Relacional com carga imprevisível, escala automática | **Aurora Serverless** |
| Alta disponibilidade / failover em outra AZ | **RDS Multi-AZ** |
| Aliviar leituras do banco principal | **Réplicas de leitura** |
| NoSQL chave-valor, serverless, escala enorme | **DynamoDB** |
| Cache em memória para acelerar qualquer banco | **ElastiCache** |
| Compatível com MongoDB | **DocumentDB** |
| Grafos / dados conectados | **Neptune** |
| Compatível com Cassandra | **Keyspaces** |
| Séries temporais / IoT | **Timestream** |
| Data warehouse / análise / BI | **Redshift** |
| Migrar banco com mínimo de inatividade | **DMS** |
| Migrar trocando de mecanismo | **SCT + DMS** |
| Controle total do banco e do SO | **Banco instalado no EC2** |

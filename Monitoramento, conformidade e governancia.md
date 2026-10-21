> Esses serviços caem no domínio de **Segurança e conformidade (30%)** e no de **Tecnologia (34%)**. A prova adora colocar **CloudWatch, CloudTrail, Config e Trusted Advisor** como alternativas na mesma questão.

### Conceitos básicos
- **Monitoramento**: acompanhar o **desempenho e a saúde** dos recursos (CPU, erros, latência) e ser **avisado** quando algo sai do normal.
- **Auditoria**: saber **quem fez o quê, quando e de onde** na conta.
- **Conformidade (compliance)**: provar que o ambiente segue **leis e normas** (LGPD, PCI DSS, HIPAA, ISO...).
- **Governança**: definir **regras e controles** para várias contas e equipes usarem a AWS de forma padronizada e segura.

### Monitoramento
#### Amazon CloudWatch
Monitora **em tempo real** os recursos e aplicações na AWS (e on-premises).
- **Métricas**: números coletados ao longo do tempo (ex.: uso de CPU de uma instância EC2, requisições no ELB).
- **Alarmes**: disparam uma ação quando uma métrica passa de um limite. Ex.: **enviar e-mail pelo SNS**, **acionar o Auto Scaling**, parar uma instância ociosa.
- **Painéis (dashboards)**: gráficos das métricas em uma tela só.
- **CloudWatch Logs**: centraliza e guarda os **logs** das aplicações e serviços.
- Também é usado para monitorar **custos** (alarme de fatura).

#### AWS CloudTrail
**Registra todas as chamadas de API** feitas na conta: **quem** fez, **o quê**, **quando**, **de onde** (IP) e se deu certo. Cada ação no Console, CLI ou SDK vira um evento.
- **Histórico de eventos**: ativado **por padrão**, guarda os últimos **90 dias** de graça.
- **Trilha (trail)**: para guardar os eventos **por mais tempo**, envia os logs para um bucket do **S3**.
- **CloudTrail Insights**: detecta **atividade incomum** de API automaticamente (ex.: pico de instâncias criadas).

### Na prova
- “Monitorar uso de CPU”, “métricas”, “alarme”, “desempenho”, “logs da aplicação” → **CloudWatch**
- “Avisar quando a CPU passar de 80%” → **alarme do CloudWatch** (+ SNS)
- “**Quem** apagou o bucket/encerrou a instância?”, “auditoria de chamadas de API”, “rastrear ações dos usuários” → **CloudTrail**
- **Pegadinha:** **CloudWatch** = **desempenho** (como está o recurso). **CloudTrail** = **auditoria** (quem fez o quê). Decore: Cloud**Trail** = **rastro** das ações.

### Conformidade
#### AWS Artifact
Portal **gratuito e self-service** com os **documentos de conformidade da própria AWS**:
- **Artifact Reports**: relatórios de auditorias externas sobre a AWS (**SOC, PCI DSS, ISO**). Usados para mostrar ao seu auditor que a **infraestrutura da AWS** é certificada.
- **Artifact Agreements**: **aceitar e gerenciar acordos** com a AWS, como o **BAA** (exigido pela **HIPAA** para dados de saúde).

#### AWS Config
**Registra a configuração** dos recursos e **o histórico de mudanças** ao longo do tempo, e verifica se estão **em conformidade** com regras que você define.
- **Regras do Config**: ex.: “todo bucket S3 deve ser privado”, “todo volume EBS deve estar criptografado”. Recursos fora da regra são marcados como **não conformes**.
- Pode **corrigir automaticamente** os recursos fora da regra.

#### AWS Audit Manager
**Audita continuamente o seu uso da AWS** e **coleta evidências automaticamente** para facilitar auditorias (com modelos prontos para normas como PCI DSS, HIPAA e GDPR).

#### Conformidade é compartilhada
- A AWS certifica a **infraestrutura** dela (centenas de programas: SOC, ISO 27001, PCI DSS, HIPAA, LGPD/GDPR...).
- O cliente é responsável pela conformidade **do que ele roda** na AWS.
- **O cliente escolhe a Região** onde os dados ficam (residência de dados), e a AWS **não move** os dados de Região sozinha.

### Na prova
- “Baixar relatórios de conformidade da AWS”, “relatório SOC/PCI/ISO”, “acordo BAA” → **Artifact**
- “Histórico de configuração do recurso”, “o que mudou na configuração”, “verificar se os recursos seguem as regras” → **Config**
- “Coletar evidências para auditoria automaticamente” → **Audit Manager**
- **Pegadinhas:**
  - **Artifact** = documentos **da AWS** (a conformidade dela). **Audit Manager** = evidências do **seu** ambiente.
  - **CloudTrail** = **quem** mudou. **Config** = **o que** mudou na configuração e se está **conforme**.

### Governança
#### AWS Organizations
Gerencia **várias contas AWS** em um só lugar.
- **Conta de gerenciamento** (*management account*): a conta principal, que cria e controla as outras (**contas-membro**).
- **Unidades organizacionais (OUs)**: agrupam contas (ex.: OU "Produção", OU "Desenvolvimento") para aplicar regras ao grupo inteiro.
- **SCPs (políticas de controle de serviço)**: definem o **máximo de permissões** que as contas podem ter. Ex.: proibir uma conta de usar certa Região ou serviço. **Valem até para o usuário-raiz** das contas-membro.
- **Faturamento consolidado** (*consolidated billing*): **uma fatura única** para todas as contas, **sem custo extra**. Soma o uso de todas as contas para alcançar **descontos por volume** e compartilha descontos de instâncias reservadas e Savings Plans.

#### AWS Control Tower
Configura e governa um **ambiente com várias contas seguindo as boas práticas** da AWS de forma **automática**. Usa o Organizations por baixo.
- **Landing zone**: o ambiente multicontas já pronto e seguro.
- **Controles (guardrails)**: regras prontas de governança (preventivas e de detecção).

#### Outros serviços de governança
| Serviço | Para quê |
| --- | --- |
| **AWS Service Catalog** | **Catálogo de produtos de TI aprovados** pela empresa (ex.: modelos de servidores, bancos). Os usuários só lançam o que está aprovado. |
| **AWS License Manager** | Gerencia **licenças de software** (Microsoft, Oracle...) e evita usar mais licenças do que a empresa tem. |
| **AWS Health Dashboard** | Mostra a **saúde dos serviços da AWS** e os eventos que afetam **os seus recursos** (falhas, manutenção programada). |
| **AWS Trusted Advisor** | Analisa a conta e dá **recomendações de boas práticas** em: **otimização de custos, desempenho, segurança, tolerância a falhas, limites de serviço e excelência operacional**. A quantidade de verificações depende do **plano de suporte**. |

### Na prova
- “Gerenciar **várias contas** de forma centralizada” → **Organizations**
- “**Uma única fatura** para várias contas”, “desconto por volume somando as contas” → **faturamento consolidado** (Organizations)
- “**Impedir** contas de usar certos serviços ou Regiões” → **SCP**
- “Criar ambiente multicontas **seguindo as boas práticas** automaticamente” → **Control Tower**
- “Usuários só podem lançar recursos **aprovados**” → **Service Catalog**
- “Controlar licenças de software” → **License Manager**
- “Algum problema da AWS está afetando **meus** recursos?”, “manutenção programada” → **Health Dashboard**
- “Recomendações para **reduzir custos** e melhorar segurança/desempenho” → **Trusted Advisor**
- **Pegadinhas:**
  - **SCP não concede** permissão, só **limita**. Quem dá a permissão continua sendo o **IAM**.
  - **Trusted Advisor** = **recomendações** (sugere melhorias). **Config** = **regras de conformidade** (diz se o recurso está conforme ou não).
  - **Organizations** = agrupar e controlar contas. **Control Tower** = montar tudo isso **pronto e com boas práticas** (usa o Organizations).

### Resumo: qual serviço escolher?
| Cenário da questão | Resposta |
| --- | --- |
| Métricas, alarmes, dashboards, logs | **CloudWatch** |
| Quem fez o quê na conta (chamadas de API) | **CloudTrail** |
| Atividade de API incomum | **CloudTrail Insights** |
| Histórico de configuração e conformidade dos recursos | **Config** |
| Relatórios de conformidade da AWS (SOC, PCI, ISO) e BAA | **Artifact** |
| Coletar evidências para auditoria | **Audit Manager** |
| Gerenciar várias contas | **Organizations** |
| Uma fatura para todas as contas | **Faturamento consolidado** |
| Limitar o que as contas podem fazer | **SCP** |
| Ambiente multicontas pronto com boas práticas | **Control Tower** |
| Catálogo de recursos aprovados | **Service Catalog** |
| Gerenciar licenças de software | **License Manager** |
| Eventos da AWS que afetam meus recursos | **Health Dashboard** |
| Recomendações de custo, segurança, desempenho, limites | **Trusted Advisor** |

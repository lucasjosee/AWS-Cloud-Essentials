> Domínio **Faturamento, preços e suporte = 12%** da prova. São quase sempre questões de **reconhecimento**: saber o **nome da ferramenta ou do plano** certo, sem fazer conta.

### Como a AWS cobra
- **Pague conforme o uso** (*pay-as-you-go*): sem contrato de longo prazo nem pagamento adiantado.
- **Pague menos ao reservar**: compromisso de 1 ou 3 anos (instâncias reservadas, Savings Plans) dá desconto.
- **Pague menos quanto mais usar**: **descontos por volume** (ex.: o preço por GB do S3 cai conforme o volume cresce).
- **Serviços gratuitos**, em que você só paga os recursos que eles criam: **IAM, VPC, Organizations (faturamento consolidado), CloudFormation, Elastic Beanstalk, Auto Scaling**.

#### O que gera custo
Os 3 principais: **computação**, **armazenamento** e **transferência de dados de saída**.
- **Dados entrando** na AWS (upload): **gratuito**.
- **Dados saindo** da AWS para a internet: **pago**.
- Dados entre **Regiões** diferentes: pago. Entre **AZs** da mesma Região: pago (valor baixo).

> **Pegadinhas:**
> - Instância EC2 **parada** não cobra computação, mas o **volume EBS continua cobrando**.
> - Modelos de preço do EC2 (sob demanda, reservadas, Savings Plans, spot, hosts dedicados) estão em [[Computação em Nuvem (EC2)]].

### Nível gratuito (Free Tier)
**Modelo antigo (contas criadas antes de 15/07/2025, e o que aparece nos simulados):** 3 tipos de oferta.
| Tipo | Como funciona | Exemplo |
| --- | --- | --- |
| **Sempre gratuito** | Não expira, vale para todos os clientes. | **Lambda**: 1 milhão de requisições/mês. DynamoDB: 25 GB. |
| **12 meses gratuitos** | Grátis por 12 meses a partir da criação da conta. | EC2: 750 h/mês de instância pequena. S3: 5 GB. |
| **Testes (trials)** | Testes gratuitos de curta duração, a partir da ativação do serviço. | SageMaker, Inspector, Lightsail |

**Modelo novo (contas criadas a partir de 15/07/2025):**
- Na criação da conta você escolhe o **plano gratuito** ou o **plano pago**.
- O plano gratuito dá **US$ 100 em créditos** + **até US$ 100 extras** ao completar atividades (ex.: lançar um EC2, criar um orçamento no Budgets). Dura **até 6 meses** ou até os créditos acabarem.
- Os serviços **sempre gratuitos** continuam existindo.

### Ferramentas de custo e faturamento
| Ferramenta | Para quê | Quando |
| --- | --- | --- |
| **AWS Pricing Calculator** | **Estimar o custo** de uma arquitetura **antes de criar** os recursos. Não precisa de conta. | **Antes** |
| **Console de Billing and Cost Management** | Ver a fatura do mês, pagar, ver o uso de cada serviço, e acessar as outras ferramentas. | Durante |
| **AWS Cost Explorer** | **Visualizar e analisar os gastos** com gráficos, por serviço, conta, Região ou tag. Mostra o histórico e **prevê** os próximos meses. | **Depois** |
| **AWS Budgets** | Definir **orçamentos** e receber **alertas** quando o gasto real ou **previsto** passar do limite. Pode executar ações automáticas. | Controle |
| **AWS Cost Anomaly Detection** | Usa **ML** para detectar **gastos fora do normal** e avisar. | Controle |
| **AWS Cost and Usage Report (CUR)** | O relatório **mais detalhado** de custo e uso (hora a hora, por recurso), enviado para um bucket S3. | Análise profunda |
| **Tags de alocação de custos** | Etiquetas nos recursos (ex.: `Projeto=Site`) para **separar os custos por equipe/projeto**. | Organização |
| **AWS Billing Conductor** | Personalizar a cobrança com **taxas próprias** (*chargeback/showback*), ex.: revendedores cobrando os clientes. | Cobrança interna |
| **AWS Compute Optimizer** | Usa **ML** para recomendar o **tamanho certo** (*right-sizing*) de EC2, EBS, Lambda e Auto Scaling. | Otimização |

**Faturamento consolidado** (pelo **AWS Organizations**): **uma fatura** para todas as contas, **sem custo extra**, somando o uso para alcançar **descontos por volume**. Ver [[Monitoramento, conformidade e governancia]].

**TCO (custo total de propriedade):** comparação do on-premises com a nuvem. Inclui custos **escondidos** do on-premises: hardware, energia, refrigeração, espaço físico, equipe de TI, licenças, troca de equipamentos.

### Na prova
- “**Estimar** o custo **antes** de migrar/criar” → **Pricing Calculator**
- “**Analisar/visualizar** os gastos passados”, “ver tendências”, “prever gastos” → **Cost Explorer**
- “**Alerta** quando o gasto passar de X dólares” → **Budgets**
- “Detectar **gastos incomuns** automaticamente” → **Cost Anomaly Detection**
- “Relatório **mais detalhado** de custos” → **Cost and Usage Report**
- “Separar custos **por departamento/projeto**” → **tags de alocação de custos**
- “**Uma fatura** para várias contas” → **faturamento consolidado (Organizations)**
- “Recomendar o **tamanho certo** das instâncias” → **Compute Optimizer**
- **Pegadinhas:**
  - **Pricing Calculator** = **antes** (estimativa). **Cost Explorer** = **depois** (análise). É a distinção que mais cai.
  - **Budgets** **avisa**. **Cost Explorer** **mostra**. O Budgets **não impede** o gasto sozinho (só se você configurar ações).

### Planos de suporte da AWS
> **Atenção:** em **02/12/2025** a AWS mudou os planos. Developer, Business e Enterprise On-Ramp **acabam em 01/01/2027**. Mas a prova CLF-C02 e os simulados ainda usam os **planos antigos**, então **decore a tabela antiga**.

#### Planos antigos (os que caem na prova)
| Plano | Preço mínimo/mês | Acesso ao suporte técnico | Caso mais grave → tempo de resposta | Trusted Advisor | TAM |
| --- | --- | --- | --- | --- | --- |
| **Basic** | **Grátis** (todos têm) | **Sem suporte técnico**: só atendimento de conta/faturamento 24/7, documentação, re:Post | — | Verificações **principais** | Não |
| **Developer** | US$ 29 | **E-mail em horário comercial** | Sistema com problema: **< 12 h** (horário comercial) | Verificações **principais** | Não |
| **Business** | US$ 100 | **24/7 por telefone, e-mail e chat** | Sistema de **produção fora do ar: < 1 h** | **Todas** | Não |
| **Enterprise On-Ramp** | US$ 5.500 | 24/7 | Sistema **crítico para o negócio** fora do ar: **< 30 min** | **Todas** | **Grupo (pool) de TAMs** |
| **Enterprise** | US$ 15.000 | 24/7 | Sistema **crítico para o negócio** fora do ar: **< 15 min** | **Todas** | **TAM designado** (exclusivo) |

- **TAM (Technical Account Manager)**: gerente técnico que conhece a sua conta, **orienta de forma proativa** e conecta você com especialistas da AWS.
- **Concierge Support**: equipe especialista em **faturamento e conta**. Só no **Enterprise On-Ramp** e no **Enterprise**.
- **Revisões do Well-Architected** e **revisões de operações** com a AWS: planos Enterprise.

#### Planos novos (desde dez/2025)
| Plano | Preço mínimo/mês | Destaques |
| --- | --- | --- |
| **Basic** | Grátis | Continua igual e é o plano padrão. |
| **Business Support+** | US$ 29 por conta | Substitui o Developer e o Business. Assistência com **IA** + acesso **24/7** a especialistas. |
| **Enterprise Support** | US$ 5.000 (antes US$ 15.000) | **TAM designado**, resposta em **15 min**, AWS Security Incident Response incluído. Clientes Enterprise On-Ramp são migrados para cá. |
| **Unified Operations** | US$ 50.000 | Plano novo mais alto: TAM + especialistas, monitoramento 24/7, resposta em **5 min** para incidentes críticos. |

### Na prova
- “Plano **mais barato** com acesso a **suporte técnico**” → **Developer**
- “**Mais barato** com suporte **24/7 por telefone**” ou “**todas** as verificações do Trusted Advisor” → **Business**
- “**TAM designado**”, “resposta em **15 min**” → **Enterprise**
- “**Pool de TAMs**”, “resposta em **30 min**” → **Enterprise On-Ramp**
- “Especialistas em **faturamento e conta** (Concierge)” → **Enterprise On-Ramp** ou **Enterprise**
- “Sem custo”, “só documentação e fóruns” → **Basic**
- **Pegadinha:** o **Basic não abre casos técnicos**, só de conta e faturamento.

### AWS Trusted Advisor
Analisa a conta e dá **recomendações de boas práticas** em **6 categorias**:
1. **Otimização de custos** (ex.: instâncias ociosas, EBS sem uso)
2. **Desempenho**
3. **Segurança** (ex.: MFA no root, portas abertas, buckets públicos)
4. **Tolerância a falhas** (ex.: snapshots, Multi-AZ)
5. **Limites de serviço** (*service quotas*)
6. **Excelência operacional**

Cada verificação mostra um status: **verde** (sem problema), **amarelo** (investigar) e **vermelho** (ação recomendada).

> **Pegadinha:** **Basic e Developer** só têm as verificações **principais** (algumas de segurança + limites de serviço). **Todas** as verificações → **Business ou superior**.

### Outros recursos de suporte
| Recurso | Para quê |
| --- | --- |
| **AWS re:Post** | **Comunidade de perguntas e respostas** (fórum) com respostas de especialistas da AWS. Gratuito. |
| **AWS Knowledge Center** | Respostas para as **perguntas mais frequentes** dos clientes. |
| **Documentação, whitepapers, AWS Prescriptive Guidance** | Guias técnicos e boas práticas. |
| **AWS Trust & Safety** | **Denunciar abuso** vindo de recursos da AWS (spam, ataques DDoS, malware, phishing). |
| **AWS Marketplace** | **Catálogo de softwares de terceiros** prontos para usar na AWS (AMIs, SaaS), com cobrança na própria fatura da AWS. |
| **AWS Partner Network (APN)** | Rede de **empresas parceiras** da AWS (consultorias e fornecedores de tecnologia) que ajudam a migrar e construir na AWS. |
| **AWS Professional Services** | **Equipe de consultoria da própria AWS** para grandes projetos (ex.: migração). |
| **AWS Solutions Architects** | Especialistas da AWS que ajudam a **desenhar a arquitetura**. |

> O **AWS IQ** (contratar freelancers certificados) **foi encerrado em 28/05/2026**. Se aparecer em simulado antigo, é essa a função dele. A AWS indica o Marketplace e a APN no lugar.

### Na prova
- “Denunciar que um IP da AWS está mandando spam/ataque” → **AWS Trust & Safety**
- “Comprar software de terceiros pronto para a AWS” → **Marketplace**
- “Empresa parceira para ajudar na migração” → **APN**
- “Fórum de perguntas da comunidade” → **re:Post**

### Resumo: qual serviço escolher?
| Cenário da questão | Resposta |
| --- | --- |
| Estimar custo antes de criar | **Pricing Calculator** |
| Analisar e prever gastos | **Cost Explorer** |
| Alerta de orçamento | **Budgets** |
| Gasto anormal detectado com ML | **Cost Anomaly Detection** |
| Relatório mais detalhado de custos | **Cost and Usage Report** |
| Separar custos por projeto | **Tags de alocação de custos** |
| Uma fatura para várias contas | **Faturamento consolidado (Organizations)** |
| Tamanho certo das instâncias | **Compute Optimizer** |
| Recomendações de custo, segurança, desempenho, limites | **Trusted Advisor** |
| Suporte técnico mais barato | **Developer** |
| Suporte 24/7 por telefone mais barato | **Business** |
| Pool de TAMs / 30 min | **Enterprise On-Ramp** |
| TAM designado / 15 min | **Enterprise** |
| Denunciar abuso | **Trust & Safety** |
| Software de terceiros | **Marketplace** |
| Parceiros de consultoria | **APN** |
| Consultoria da própria AWS | **Professional Services** |

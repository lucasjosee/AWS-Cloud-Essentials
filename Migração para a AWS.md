> Migração cai no domínio **Conceitos de nuvem (24%)**: **CAF** e as **7 Rs**. Os serviços de migração caem no domínio de **Tecnologia (34%)**.

### AWS Cloud Adoption Framework (AWS CAF)
Conjunto de **orientações e boas práticas da AWS** para ajudar a empresa a **planejar a adoção da nuvem**. Organiza o que precisa ser feito em **6 perspectivas**, cada uma com seus responsáveis.

#### As 6 perspectivas
| Perspectiva | Foco | Quem participa |
| --- | --- | --- |
| **Negócios** | Garantir que a nuvem traga **resultados para o negócio** (investimento, retorno, estratégia). | CEO, CFO, COO, CIO |
| **Pessoas** | **Cultura, treinamento**, novas funções e **mudança organizacional**. Ex.: capacitar a equipe para a nuvem. | RH, gestores de pessoas |
| **Governança** | **Gerenciar riscos**, orçamento e conformidade, maximizando os benefícios. | CIO, CFO, gestores de projeto |
| **Plataforma** | **Arquitetura** e infraestrutura da nuvem: computação, rede, armazenamento, padrões técnicos. | CTO, arquitetos, engenheiros |
| **Segurança** | **Proteger dados e cargas de trabalho** (confidencialidade, integridade, disponibilidade). | CISO, equipe de segurança |
| **Operações** | Manter os serviços **funcionando no dia a dia**: monitoramento, incidentes, continuidade. | Equipe de operações de TI, suporte |

**Truque para decorar:** Negócios, Pessoas e Governança = **lado do negócio**. Plataforma, Segurança e Operações = **lado técnico**.

#### Fases da jornada de transformação na nuvem
1. **Prever (Envision)**: mostrar **como a nuvem ajuda o negócio** e identificar as oportunidades.
2. **Alinhar (Align)**: identificar **lacunas de capacidade** entre as 6 perspectivas e alinhar as partes interessadas.
3. **Lançar (Launch)**: entregar **projetos-piloto** em produção e mostrar valor aos poucos.
4. **Escalar (Scale)**: **expandir** os pilotos para toda a empresa.

#### Domínios de transformação
**Tecnologia → Processo → Organização → Produto**: a nuvem muda primeiro a tecnologia, depois os processos de negócio, a forma como a empresa se organiza e, por fim, os produtos e serviços que ela oferece.

#### Benefícios esperados do CAF
**Reduzir riscos do negócio**, **melhorar o desempenho ESG** (ambiental, social e de governança), **aumentar a receita** e **aumentar a eficiência operacional**.

### Na prova
- “**Treinamento** da equipe”, “**cultura**”, “**mudança organizacional**”, “novas funções” → perspectiva de **Pessoas**
- “**Retorno do investimento**”, “estratégia de negócio”, “resultados financeiros” → **Negócios**
- “**Riscos**”, “**conformidade**”, “orçamento”, “gestão do portfólio” → **Governança**
- “**Arquitetura**”, “infraestrutura”, “provisionar recursos” → **Plataforma**
- “**IAM**”, “**detecção de ameaças**”, “**proteção de dados**” → **Segurança**
- “**Monitoramento**”, “**incidentes**”, “manter funcionando” → **Operações**
- “Orientação/boas práticas para **planejar a migração** da empresa toda” → **AWS CAF**
- **Pegadinha:** **Governança** = gerenciar **riscos e conformidade** do negócio. **Segurança** = **controles técnicos** (IAM, criptografia, detecção).

### As 7 Rs (estratégias de migração)
| Estratégia | O que faz | Esforço | Exemplo |
| --- | --- | --- | --- |
| **Redefinir a hospedagem** (*Rehost*, "lift and shift") | Move a aplicação **do jeito que está**, sem mudanças. | Baixo, **mais rápida** | Servidores on-premises → instâncias EC2 |
| **Realocar** (*Relocate*) | Move servidores inteiros no nível do **hypervisor**, sem comprar hardware novo nem mudar a aplicação. | Baixo | VMware on-premises → **VMware Cloud on AWS** |
| **Redefinir a plataforma** (*Replatform*, "lift, tinker and shift") | Move com **pequenas otimizações**, sem mudar o código principal. | Baixo–médio | MySQL instalado em servidor → **Amazon RDS** |
| **Recomprar** (*Repurchase*, "drop and shop") | **Troca** a aplicação por um produto novo, geralmente **SaaS**. | Médio | CRM próprio → **Salesforce** |
| **Refatorar / rearquitetar** (*Refactor*) | **Reescreve** a aplicação para ser **nativa da nuvem**. | **Alto**, maior benefício | Monolito → microsserviços com Lambda e DynamoDB |
| **Reter** (*Retain*) | **Mantém on-premises** por enquanto (revisitar depois). | Nenhum agora | App que vai ser descontinuado em breve, ou com exigência de conformidade |
| **Retirar** (*Retire*) | **Desliga** aplicações que não são mais necessárias. | Baixo | Apps sem uso ou duplicados descobertos na análise |

### Na prova
- “**Mais rápido**”, “**sem alterações**”, “lift and shift” → **Rehost**
- “Pequenas otimizações”, “passar o banco para um **serviço gerenciado**” → **Replatform**
- “Migrar **VMware**” → **Relocate**
- “Trocar por **SaaS**”, “comprar outro produto” → **Repurchase**
- “Reescrever para **nativo da nuvem**”, “**serverless**”, “microsserviços”, “máxima escalabilidade” → **Refactor**
- “Ainda **não está pronto** para migrar”, “manter no local” → **Retain**
- “Aplicação **sem uso**”, “desligar” → **Retire**
- **Pegadinhas:**
  - **Rehost** muda **nada**. **Replatform** muda **pouco** (sem mexer no código). **Refactor** muda **muito** (reescreve).
  - **Retain** = **fica** on-premises. **Retire** = **desliga** de vez.

### Serviços de migração
#### Planejamento e acompanhamento
| Serviço | Para quê |
| --- | --- |
| **AWS Migration Hub** | **Painel central** para **acompanhar o progresso** de todas as migrações, em várias ferramentas. |
| **AWS Application Discovery Service** | **Descobre** os servidores on-premises, o uso deles e as **dependências** entre aplicações, para planejar a migração. |
| **Migration Evaluator** | Monta o **caso de negócio**: estima quanto a empresa **economizaria** migrando (custo atual × AWS). |
| **AWS Transform** | Serviço **novo, com agentes de IA**, que automatiza a descoberta, o planejamento e a **modernização** (ex.: VMware, .NET, mainframe). |

> O **Migration Hub** e o **Application Discovery Service** **não aceitam clientes novos desde 07/11/2025**. A AWS indica o **AWS Transform** no lugar. Na prova, os dois ainda aparecem com as funções acima.

#### Migração de servidores e bancos
- **AWS Application Migration Service (MGN)**: **replica servidores** (físicos, VMware, outras nuvens) para o EC2 de forma automática. É a ferramenta principal de **Rehost (lift and shift)**.
- **AWS Database Migration Service (DMS)**: migra **bancos de dados** com o **mínimo de inatividade** (o banco de origem continua funcionando). Também faz **replicação contínua**.
- **AWS Schema Conversion Tool (SCT)**: **converte o esquema** quando o banco **muda de mecanismo** (ex.: Oracle → Aurora). Ver [[Banco de Dados]].
- **AWS Mainframe Modernization**: migra e moderniza aplicações de **mainframe**.

#### Transferência de dados
| Serviço | Como | Quando usar |
| --- | --- | --- |
| **AWS DataSync** | **Online** (pela rede) | Copiar/sincronizar arquivos (NFS, SMB) de servidores on-premises para **S3, EFS ou FSx**, de forma automatizada. |
| **AWS Transfer Family** | **Online** | Receber arquivos por **SFTP, FTPS ou FTP** direto no S3 ou EFS. |
| **AWS Storage Gateway** | **Híbrido** | Apps on-premises continuam usando armazenamento da AWS. Ver [[Armazenamento]]. |
| **Família AWS Snow** (**Snowball Edge**) | **Offline** (dispositivo físico) | **Muitos dados** (TB/PB) e **internet lenta ou cara**. A AWS manda o aparelho, você copia os dados e devolve. |

> Snowcone e Snowmobile foram **descontinuados em 2024**, e o Snowball Edge **não aceita clientes novos desde nov/2025**. Mesmo assim ainda estão no guia do CLF-C02 (detalhes em [[Armazenamento]]).

#### Programas de apoio
- **AWS Migration Acceleration Program (MAP)**: programa da AWS com **metodologia, ferramentas, especialistas, parceiros e créditos** para ajudar empresas a migrar. Segue 3 fases: **Avaliar** (*Assess*) → **Mobilizar** (*Mobilize*) → **Migrar e modernizar** (*Migrate and Modernize*).

### Na prova
- “**Acompanhar** o progresso das migrações em um só lugar” → **Migration Hub**
- “**Descobrir** servidores e **dependências** on-premises” → **Application Discovery Service**
- “Estimar a **economia** de migrar”, “caso de negócio” → **Migration Evaluator**
- “Migrar **servidores** para EC2 (lift and shift) automaticamente” → **Application Migration Service (MGN)**
- “Migrar **banco de dados** com mínimo de inatividade” → **DMS**
- “Banco **mudando de mecanismo**” → **SCT + DMS**
- “Transferir arquivos **pela rede** de forma automatizada” → **DataSync**
- “Clientes enviam arquivos por **SFTP**” → **Transfer Family**
- “**Petabytes** de dados com **internet lenta**”, “transferência offline” → **Snowball Edge**
- “Programa da AWS com **especialistas e créditos** para migrar” → **MAP**
- **Pegadinhas:**
  - **DataSync** = **online** (pela rede). **Snowball** = **offline** (aparelho físico).
  - **MGN** migra **servidores**. **DMS** migra **bancos de dados**.
  - Para calcular quanto tempo leva pela rede: se a transferência levaria **semanas ou meses**, a resposta é **Snowball**.

### Resumo: qual serviço escolher?
| Cenário da questão | Resposta |
| --- | --- |
| Boas práticas para planejar a adoção da nuvem | **AWS CAF** |
| Mover sem mudanças, o mais rápido possível | **Rehost** |
| Mover com pequenas otimizações | **Replatform** |
| Reescrever como nativo da nuvem | **Refactor** |
| Trocar por SaaS | **Repurchase** |
| Mover VMware sem mudanças | **Relocate** |
| Manter on-premises | **Retain** |
| Desligar | **Retire** |
| Acompanhar migrações | **Migration Hub** |
| Descobrir servidores e dependências | **Application Discovery Service** |
| Caso de negócio / economia estimada | **Migration Evaluator** |
| Migrar servidores para EC2 | **Application Migration Service (MGN)** |
| Migrar banco de dados | **DMS** (+ **SCT** se trocar de mecanismo) |
| Transferência online automatizada | **DataSync** |
| SFTP/FTP para a AWS | **Transfer Family** |
| Transferência offline de muitos dados | **Snowball Edge** |
| Programa de apoio com créditos e especialistas | **MAP** |

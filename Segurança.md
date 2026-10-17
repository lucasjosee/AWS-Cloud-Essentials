> Segurança e conformidade é o **maior domínio da prova: 30%** das questões.

### Conceitos básicos
- **Autenticação**: provar **quem você é** (login + senha + MFA).
- **Autorização**: definir **o que você pode fazer** depois de entrar (permissões).
- **Negar por padrão**: na AWS, tudo começa **negado**. Só funciona o que for permitido explicitamente.
- **Privilégio mínimo** (*least privilege*): dar **só as permissões necessárias** para a tarefa, nada além.
- Controles de segurança da AWS seguem 3 ideias: **prevenir** acessos indevidos, **proteger** redes e dados, **detectar e responder** a incidentes.

### Modelo de responsabilidade compartilhada
(ver também [[Compreensão dos Serviços Computacionais]])
- **AWS = segurança DA nuvem**: data centers, hardware, rede física, hypervisor, infraestrutura global.
- **Cliente = segurança NA nuvem**: dados, **usuários e permissões do IAM**, criptografia, grupos de segurança, configuração dos serviços e, no EC2, **patches do SO convidado**.
- Quanto mais gerenciado o serviço (EC2 → RDS → Lambda), menos o cliente faz. Mas **dados e IAM são sempre do cliente**.

### Na prova
- “Segurança física do data center”, “hardware”, “hypervisor” → **AWS**
- “Patches do SO do EC2”, “regras do grupo de segurança”, “permissões do IAM”, “criptografar os dados” → **cliente**
- “Patches do banco no RDS” → **AWS**. “Usuários do banco no RDS” → **cliente**
- **Pegadinha:** **treinamento dos funcionários** do cliente e **classificação dos dados** são responsabilidade do **cliente**.

### AWS Identity and Access Management (IAM)
Serviço **gratuito** e **global** (não é por Região) que controla **quem** acessa **o quê** na conta AWS.

#### Usuário-raiz (root user)
- Criado junto com a conta, usando o **e-mail** do cadastro. Tem **acesso total e irrestrito**, e não dá para limitar as permissões dele com políticas do IAM.
- Boas práticas: **ativar MFA imediatamente**, **não usar no dia a dia** e **não criar chaves de acesso** para ele. Use-o só para criar o primeiro usuário administrador e para as tarefas que só ele pode fazer.
- **Tarefas que só o usuário-raiz pode fazer** (caem na prova):
  - **Encerrar a conta** AWS
  - **Alterar o plano de suporte** (AWS Support)
  - Alterar configurações da conta (e-mail, senha do root, nome da conta)
  - Restaurar permissões de um administrador do IAM que se bloqueou

#### Identidades do IAM
| Identidade | O que é | Detalhe importante |
| --- | --- | --- |
| **Usuário do IAM** | Uma **pessoa ou aplicação**, com credenciais **permanentes** (senha e/ou chaves de acesso). | Começa **sem nenhuma permissão**. Um usuário por pessoa, **nunca compartilhar**. |
| **Grupo do IAM** | Conjunto de usuários que recebem as **mesmas permissões**. Ex.: grupo "Desenvolvedores". | Facilita a gestão. Grupo **não** pode conter outro grupo e **não** dá para fazer login como grupo. |
| **Função (role) do IAM** | Identidade com **credenciais temporárias**, que alguém ou algum serviço **assume** quando precisa. | Usada por **serviços da AWS** (ex.: EC2 acessando o S3), acesso **entre contas** e **usuários federados**. |
| **Política do IAM** | Documento **JSON** que **permite ou nega** ações em recursos (`Effect`, `Action`, `Resource`). | **Negação explícita sempre vence** uma permissão. |

#### MFA (autenticação multifator)
Além da senha, pede um **segundo fator** (código de um app no celular ou chave física). Ativar sempre no **usuário-raiz** e em usuários com muitas permissões.

### Na prova
- “Primeira coisa a fazer depois de criar a conta” → **ativar MFA no usuário-raiz**
- “Aplicação no **EC2** precisa acessar o **S3**” → **função (role) do IAM**, **nunca** chaves de acesso gravadas no código
- “Dar as mesmas permissões para vários funcionários” → **grupo do IAM**
- “Acesso temporário”, “acesso entre contas” → **função (role)**
- “Dar só o necessário” → **privilégio mínimo**
- “Encerrar a conta” ou “mudar o plano de suporte” → **usuário-raiz**

### Outros serviços de gerenciamento de acesso
- **AWS IAM Identity Center** (antigo *AWS SSO*): **login único (SSO)** para **várias contas AWS** e aplicações, usando um só usuário. Pode usar o diretório que a empresa já tem (ex.: Active Directory). Palavra-chave: “funcionários acessando várias contas com o mesmo login”.
- **AWS Secrets Manager**: guarda **segredos** (senhas de banco, chaves de API) e faz a **rotação automática** deles. Palavra-chave: “não deixar senha no código”, “rotacionar senhas automaticamente”.
- **AWS Systems Manager**: visão e automação centralizada dos servidores (EC2 e on-premises). Ex.: aplicar **patches** em muitos servidores de uma vez.

### Proteção de rede e aplicações
**Ataque DDoS** (negação de serviço distribuída): muitas máquinas enviam tráfego ao mesmo tempo para derrubar a aplicação.

Defesas que já vimos: **grupos de segurança** (só deixam passar o tráfego permitido, ver [[Rede]]) e **ELB** (distribui o tráfego para nenhum servidor ficar sobrecarregado).

- **AWS Shield**: proteção contra **DDoS**.
  - **Shield Standard**: **gratuito e automático** para todos os clientes. Protege contra os ataques mais comuns (camadas 3 e 4).
  - **Shield Advanced**: **pago**. Diagnósticos detalhados, proteção contra ataques mais sofisticados, acesso à **equipe de resposta a DDoS** da AWS e **proteção de custos** (reembolso de gastos extras de escalabilidade causados pelo ataque).
- **AWS WAF** (*Web Application Firewall*): filtra requisições **web (HTTP/HTTPS, camada 7)** com regras, numa **Web ACL**. Bloqueia **SQL injection**, **cross-site scripting (XSS)** e **IPs ou países** específicos. Funciona com **CloudFront, Application Load Balancer e API Gateway**.

### Na prova
- “Proteção contra DDoS sem custo extra” → **Shield Standard**
- “Equipe especializada em DDoS 24/7”, “proteção avançada contra DDoS” → **Shield Advanced**
- “**SQL injection**”, “**XSS**”, “bloquear IPs/países no site” → **WAF**
- **Pegadinha:** Shield = **DDoS**. WAF = **ataques à aplicação web**. Grupo de segurança **não** bloqueia SQL injection.

### Proteção de dados (criptografia)
- **Criptografia em repouso**: dados **guardados** (disco, S3, banco).
  - **S3**: criptografa **todos os objetos novos por padrão**.
  - **DynamoDB**: criptografado em repouso **por padrão**.
  - **EBS**: suporta criptografia de volumes e snapshots (você ativa).
- **Criptografia em trânsito**: dados **viajando** pela rede, protegidos com **SSL/TLS (HTTPS)**.

| Serviço | Para quê |
| --- | --- |
| **AWS KMS** (*Key Management Service*) | **Criar e gerenciar chaves de criptografia** e controlar quem pode usá-las. As chaves **nunca saem** do KMS. Integrado à maioria dos serviços. |
| **AWS CloudHSM** | Módulo de hardware (HSM) **dedicado só para você**, para quem precisa de **controle total das chaves** por exigência de conformidade. |
| **AWS Certificate Manager (ACM)** | Cria, gerencia e **renova automaticamente** certificados **SSL/TLS** (HTTPS). Certificados públicos são **gratuitos**. |
| **Amazon Macie** | Usa **ML** para **descobrir dados sensíveis (PII)** como CPF, cartão de crédito e endereços **no S3**. |

### Na prova
- “Criar e gerenciar chaves de criptografia” → **KMS**
- “HSM dedicado”, “controle exclusivo das chaves por conformidade” → **CloudHSM**
- “Certificado SSL/TLS”, “HTTPS” → **ACM**
- “Encontrar dados pessoais/sensíveis no **S3**” → **Macie**
- **Pegadinha:** **ativar a criptografia** e **gerenciar o acesso às chaves** é responsabilidade do **cliente**.

### Detecção e resposta a incidentes
| Serviço | O que faz | Palavra-chave |
| --- | --- | --- |
| **Amazon Inspector** | **Escaneia vulnerabilidades** de software e exposição de rede em **EC2, imagens de contêiner (ECR) e Lambda**, automaticamente. | “vulnerabilidades”, “CVEs”, “software desatualizado” |
| **Amazon GuardDuty** | **Detecção de ameaças** contínua com ML, analisando logs da conta e da rede (CloudTrail, VPC Flow Logs, DNS). Não afeta o desempenho dos recursos. | “atividade suspeita/maliciosa”, “conta comprometida”, “mineração de criptomoeda” |
| **Amazon Detective** | **Investiga a causa raiz** de um problema de segurança, mostrando visualmente as interações entre recursos e usuários. | “investigar”, “causa raiz” |
| **AWS Security Hub** | **Painel único** que reúne os alertas de GuardDuty, Inspector, Macie etc. e verifica a conformidade com boas práticas. | “visão centralizada da segurança”, “todos os alertas em um só lugar” |

> **Para decorar:** Guard**Duty** **vigia** ameaças, **Inspector** **inspeciona** vulnerabilidades, **Macie** cuida dos **dados sensíveis**, **Detective** **investiga** a causa, **Security Hub** **junta** tudo.

### Na prova
- **Pegadinhas:**
  - **Inspector** procura **falhas no seu software** (antes de alguém explorar). **GuardDuty** procura **ataques/comportamento suspeito** acontecendo.
  - **GuardDuty** **detecta** a ameaça. **Detective** **investiga** a causa depois.
  - **Macie** é só para **dados sensíveis no S3**, não detecta ataques.

### Recursos de segurança da AWS
- **AWS Knowledge Center**: perguntas frequentes e solução de problemas.
- **Documentação de segurança** de cada serviço e **AWS Security Blog**.
- **AWS Marketplace**: catálogo de **softwares de segurança de terceiros** (firewalls, antivírus etc.) prontos para usar na AWS.

### Resumo: qual serviço escolher?
| Cenário da questão | Resposta |
| --- | --- |
| Controlar quem acessa o quê na conta | **IAM** |
| Permissões temporárias / serviço acessando outro serviço | **Função (role) do IAM** |
| Login único em várias contas AWS | **IAM Identity Center** |
| Guardar e rotacionar senhas e chaves de API | **Secrets Manager** |
| Aplicar patches em vários servidores | **Systems Manager** |
| Proteção DDoS gratuita | **Shield Standard** |
| Proteção DDoS avançada + equipe de resposta | **Shield Advanced** |
| Bloquear SQL injection / XSS / IPs na aplicação web | **WAF** |
| Gerenciar chaves de criptografia | **KMS** |
| Hardware dedicado para chaves | **CloudHSM** |
| Certificados SSL/TLS | **ACM** |
| Achar dados sensíveis no S3 | **Macie** |
| Escanear vulnerabilidades em EC2/contêineres/Lambda | **Inspector** |
| Detectar ameaças e atividade maliciosa | **GuardDuty** |
| Investigar a causa raiz de um incidente | **Detective** |
| Painel central de alertas de segurança | **Security Hub** |

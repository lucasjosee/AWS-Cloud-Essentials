### Amazon EC2
Serviço de computação em nuvem da AWS, é através dele que podemos alugar poder computacional para diversos usos.

Essa locação, é feita através de instâncias que são maquinas virtuais hospedadas em  nuvem. Varias instâncias podem rodar em um mesmo data-center na AWS (**multilocação**: várias VMs compartilham o mesmo hardware, isoladas pelo hypervisor), e o poder computacional delas podem ser configuradas de acordo com a necessidade do cliente. O acesso a essas VMs podem ser feitos através por meio do Console Web da AWS, AWS CLI e SDKs.

- EC2 é **IaaS / não gerenciado**: o cliente cuida do **SO convidado** (patches, atualizações), apps, dados e grupos de segurança. A AWS cuida do hardware e do **hypervisor**.
- **AMI (Amazon Machine Image)**: o "molde" da instância (SO + softwares). Usada para lançar instâncias iguais.

### Tipos de Instâncias
![[Pasted image 20261006091954.png]]

| Família | Quando usar (palavra-chave na prova) |
| --- | --- |
| **Uso geral** | Equilíbrio de CPU, memória e rede. Servidores web, repositórios de código. |
| **Otimizada para computação** | Muito processamento: servidores de jogos, HPC, modelagem científica, processamento em lote. |
| **Otimizada para memória** | Grandes conjuntos de dados **na memória**: bancos de dados em memória, análises em tempo real. |
| **Computação acelerada** | Usa **GPU/aceleradores de hardware**: machine learning, gráficos, cálculos de ponto flutuante. |
| **Otimizada para armazenamento** | Muita leitura/escrita em **armazenamento local** (alto IOPS): data warehouses, bancos OLTP. |

### Precificação
![[Pasted image 20261006092111.png]]

| Modelo | Como funciona | Quando usar |
| --- | --- | --- |
| **Sob demanda** | Paga por segundo/hora, sem compromisso, sem desconto. | Cargas **curtas, imprevisíveis**, que não podem ser interrompidas, testes. |
| **Instâncias reservadas (RI)** | Compromisso de **1 ou 3 anos**, até ~72% de desconto. Pagamento total, parcial ou nenhum adiantado (quanto mais adiantado, maior o desconto). *Standard* (maior desconto) ou *Convertible* (pode trocar família/SO). | Cargas **estáveis e previsíveis** (ex.: banco de dados 24/7). |
| **Savings Plans** | Compromisso de **gasto por hora ($/h) por 1 ou 3 anos**, até ~72%. Mais flexível que RI: vale também para **Lambda e Fargate**. | Uso constante, mas querendo flexibilidade de tipo de instância/região. |
| **Instâncias spot** | Usa capacidade ociosa da AWS, até **90%** de desconto. A AWS pode retomar a instância com **aviso de 2 minutos**. | Cargas **tolerantes a interrupção** e flexíveis: processamento em lote, análise de dados. |
| **Hosts dedicados** | Servidor físico **inteiro** só seu. O mais caro. | **Licenças de software por servidor/núcleo** (BYOL) ou exigências de conformidade. |
| **Instâncias dedicadas** | Hardware não compartilhado com outros clientes, mas você não controla o servidor físico. | Isolamento de hardware sem precisar de visibilidade do servidor. |

> **Pegadinhas:**
> - "Mais barato possível + pode ser interrompido" → **Spot**. "Não pode ser interrompido + curto prazo" → **Sob demanda**.
> - "Usar licenças existentes vinculadas ao servidor" → **Host dedicado** (não instância dedicada).
> - Reserved Instance é **desconto na fatura**, não garante capacidade. Para garantir capacidade numa AZ: **Reservas de capacidade sob demanda**.

### Escalando o EC2
**Escalabilidade x Elasticidade:**
	Escalabilidade refere-se a capacidade da operação designar mais recursos conforme a demanda aumenta. Isso pode acontecer aumentando o poder computacional de cada instância (vertical / *scale up*), ou designando mais instâncias para a operação (horizontal / *scale out*). Agora, uma operação ser elástica significa que ela vai se adaptar a demanda que possui ao longo do tempo, **aumentando e diminuindo** automaticamente.

A AWS cuida da escalabilidade usando o EC2 Auto Scaling, esse serviço usa outros serviços da própria AWS como o Amazon CloudWatch para monitorar as métricas necessárias. Ele oferece duas abordagens. O _escalonamento dinâmico_ se ajusta em tempo real às flutuações na demanda. O _escalonamento preditivo_ agenda preventivamente o número certo de instâncias com base na demanda prevista.

Um **grupo de Auto Scaling** é configurado com três números:
- **Capacidade mínima**: quantas instâncias sempre existem.
- **Capacidade desejada**: quantas devem existir agora (se não informada, = mínima).
- **Capacidade máxima**: limite para não estourar o custo.

### Elastic Load Balancing
O serviço Elastic Load Balancing (ELB) opera distribuindo de forma automática o fluxo de tráfego de uma aplicação entre diversos recursos, como as instâncias EC2, com o objetivo de otimizar o desempenho e a confiabilidade. O balanceador de carga atua como um ponto centralizado de entrada para todo o tráfego web destinado a um grupo de Auto Scaling. À medida que a quantidade de instâncias EC2 varia em resposta à demanda, todas as requisições são inicialmente recebidas pelo balanceador de carga. A partir daí, o tráfego é distribuído uniformemente pelas instâncias disponíveis.

O ELB e o Auto Scaling são serviços independentes, mas para garantir uma escalabilidade adequada é recomendando o uso dos dois em conjunto. O ELB é **regional** e distribui tráfego entre várias **AZs** (alta disponibilidade).

##### Os tipos de load balancer

| Tipo                                | Camada         | Quando usar                                             |
| ----------------------------------- | -------------- | ------------------------------------------------------- |
| **Application Load Balancer (ALB)** | 7 (HTTP/HTTPS) | Sites, APIs, microsserviços. O mais usado.              |
| **Network Load Balancer (NLB)**     | 4 (TCP/UDP)    | Altíssimo desempenho, jogos online, IP fixo.            |
| **Gateway Load Balancer (GWLB)**    | 3 (IP)         | Passar tráfego por firewalls e appliances de segurança. |
| **Classic Load Balancer**           | 4 e 7          | Legado; não se usa em projetos novos.                   |

### Mensagens e desacoplamento
Arquitetura **fortemente acoplada**: se um componente falha, os outros falham junto. **Fracamente acoplada** (*loosely coupled*): componentes se comunicam por mensagens, e a falha de um não derruba os outros. A prova adora esse conceito.

| Serviço | O que faz | Palavra-chave |
| --- | --- | --- |
| **Amazon SQS** | **Fila** de mensagens. Guarda a mensagem até o consumidor processar. | "desacoplar", "fila", "não perder mensagens" |
| **Amazon SNS** | **Publicar/assinar (pub/sub)**. Envia uma mensagem para vários assinantes (e-mail, SMS, Lambda, SQS). | "notificação", "enviar para vários" |
| **Amazon EventBridge** | Barramento de eventos que conecta apps e serviços (inclusive SaaS) por eventos. | "orientado a eventos", "integrar apps SaaS" |

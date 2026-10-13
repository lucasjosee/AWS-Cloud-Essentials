Serviços gerenciáveis são como maquinas de café de capsulas, são simples de usar mas são menos personalizáveis, e muitas responsabilidades são transferidas para a AWS. Já serviços não gerenciáveis, são mais "difíceis" de serem usados e o usuário também possui suas responsabilidade, o EC2 é um serviço desse tipo e a responsabilidade do usuário é a segurança na nuvem. 

![[Pasted image 20261007215259.png]]

**Modelo de responsabilidade compartilhada** (cai muito!):
- **AWS = segurança DA nuvem**: hardware, data centers, rede física, hypervisor, infraestrutura global.
- **Cliente = segurança NA nuvem**: dados, permissões do IAM, criptografia, grupos de segurança, e (no EC2) o SO convidado e seus patches.
- Quanto mais gerenciado o serviço, menos o cliente faz. Mas **dados e permissões do IAM são sempre do cliente**, em qualquer serviço.

Server less -> serviços que os usuários não precisam usar nenhum servidor, a AWS cuida da maioria das coisas.

#### AWS Lambda
Serviço que executa códigos em resposta a triggers (eventos) sem necessidade de mexer com servidor ou instancias. A AWS faz tudo, desde cuidar da escalabilidade ou disponibilidade. A cobrança do serviço é feita de acordo com o tempo computacional consumido (em milissegundos) e número de requisições — **não paga nada quando o código não está rodando**.

Embora a AWS gerencie a infraestrutura, a equipe é responsável por configurar corretamente as funções e permissões da solução AWS Identity and Access Management (IAM) para qualquer serviço que a função Lambda acesse.

> **Pegadinha:** Lambda roda no máximo **15 minutos** por execução. Processamento longo e contínuo → EC2/ECS, não Lambda.

### Contêineres na Nuvem
Contêiner = aplicação + tudo que ela precisa para rodar, num pacote portátil (ex.: Docker). Cada VM/instância pode rodar vários contêineres, portanto, caso um microserviço está sendo requisitado mais que outros, os orquestradores de contêineres da AWS vão subir mais contêineres daquele microserviço, aumentando a disponibilidade. Caso uma instancia seja sobrecarregada, o EC2 Auto Scaling sobe mais instancias para lidar com as requisições, e assim por diante. 

##### Amazon ECS
A solução Amazon Elastic Container Service (Amazon ECS) é um serviço escalável de orquestração de contêineres **da própria AWS** que executa e gerencia contêineres na AWS, como contêineres Docker.

##### Amazon EKS
A solução Amazon Elastic Kubernetes Service (Amazon EKS) é um serviço totalmente gerenciado para executar o **Kubernetes** (código aberto) na AWS.

Os dois podem rodar de duas formas:
- **Com EC2**: você gerencia as instâncias por baixo → mais controle.
- **Com Fargate**: sem servidor, a AWS gerencia a infraestrutura → menos trabalho.

> **Pegadinha:** a pergunta falar em "Kubernetes" → **EKS**. Falar em "orquestrar Docker" sem Kubernetes → **ECS**.

##### Amazon ECR
O **ECR (Elastic Container Registry)** é um **registro de imagens**: um repositório onde as imagens ficam armazenadas para serem baixadas depois. Por padrão, os repositórios são **privados**, ou seja, só quem tem permissão na sua conta AWS consegue acessar.

##### Fargate
**Mecanismo de computação sem servidor para contêineres**. Funciona com ECS e EKS, dando a liberdade do usuário não precisar configurar servidores/instâncias, a AWS faz isso tudo sozinha, dando liberdade para que o desenvolvedor foque em desenvolver suas aplicações.

#### Resumo: qual serviço de computação escolher?
| Cenário da questão | Resposta |
| --- | --- |
| Controle total do SO / servidor | **EC2** |
| Rodar código em resposta a eventos, sem servidor, pagar só pela execução | **Lambda** |
| Contêineres sem gerenciar servidores | **Fargate** (com ECS ou EKS) |
| Contêineres com Kubernetes | **EKS** |
| Subir app web só enviando o código (PaaS) | **Elastic Beanstalk** |
| VPS simples com preço mensal fixo | **Lightsail** |
| Trabalhos em lote em grande escala | **AWS Batch** |
| Serviços AWS dentro do data center do cliente | **Outposts** |

[[Outros Serviços AWS...]]

Serviços gerenciáveis são como maquinas de café de capsulas, são simples de usar mas são menos personalizáveis, e muitas responsabilidades são transferidas para a AWS. Já serviços não gerenciáveis, são mais "difíceis" de serem usados e o usuário também possui suas responsabilidade, o EC2 é um serviço desse tipo e a responsabilidade do usuário é a segurança na nuvem. 

![[Pasted image 20261007215259.png]]

Server less -> serviços que os usuários não precisam usar nenhum servidor, a AWS cuida da maioria das coisas.

#### AWS Lambda
Serviço que executa códigos em resposta a triggers sem necessidade de mexer com servidor ou instancias. A AWS faz tudo, desde cuidar da escalabilidade ou disponibilidade. A cobrança do serviço é feita de acordo com o tempo computacional consumido.

Embora a AWS gerencie a infraestrutura, a equipe é responsável por configurar corretamente as funções e permissões da solução AWS Identity and Access Management (IAM) para qualquer serviço que a função Lambda acesse.

### Contêineres na Nuvem
Aqui, a disponibilidade para o serviço aumenta ainda mais. Cada VM/instância pode rodar vários contêineres, portanto, caso um microserviço está sendo requisitado mais que outros, os orquestradores de contêineres da AWS vão subir mais contêineres daquele microserviço na VM, aumentando a disponibilidade. Caso uma instancia seja sobrecarregada, o EC2 auto Scaling sobre mais instancias para lidar com as requisições, e assim por diante. 

##### Amazon ECS
A solução Amazon Elastic Container Service (Amazon ECS) é um serviço escalável de orquestração de contêineres que executa e gerencia contêineres na AWS, como contêineres Docker.

- **O Amazon ECS com Amazon EC2** é ideal para empresas de pequeno a médio porte que precisam de controle total sobre a infraestrutura. Adequado para aplicações personalizadas que exigem configurações específicas de hardware ou rede, com a flexibilidade do Amazon EC2 e a simplicidade do Amazon ECS.

- **O Amazon ECS with AWS Fargate** é perfeito para startups ou pequenas equipes que criam aplicativos web com tráfego variável. É uma opção com tecnologia sem servidor, sem necessidade de gerenciamento de servidores, para que as equipes possam se concentrar no desenvolvimento enquanto o Amazon ECS lida com o scaling e a orquestração.

##### **Amazon EKS**
A solução Amazon Elastic Kubernetes Service (Amazon EKS) é um serviço totalmente gerenciado para executar o Kubernetes na AWS. Ela simplifica a implantação, o gerenciamento e o escalonamento de aplicações em contêineres com o uso do Kubernetes de código aberto, com suporte e atualizações contínuos da comunidade em geral.

- **Amazon EKS com Amazon EC2:** Isso é melhor para empresas que precisam de controle total sobre a infraestrutura. Ele oferece uma personalização profunda das instâncias do EC2 junto com a escalabilidade do Kubernetes, ideal para workloads complexas e de grande escala.

- **Amazon EKS com AWS Fargate:** Isso é ótimo para equipes que desejam flexibilidade no Kubernetes sem gerenciar servidores. Ele combina a potência do Kubernetes com a simplicidade com tecnologia sem servidor, ajudando a escalar aplicações rapidamente em vários casos de uso.


##### Amazon ECR
O **ECR (Elastic Container Registry)** é um **registro de imagens**: um repositório onde as imagens ficam armazenadas para serem baixadas depois. Por padrão, os repositórios são **privados**, ou seja, só quem tem permissão na sua conta AWS consegue acessar.


##### Fargate
Funciona com as soluções ECS e EKS, dando a liberdade do usuário não precisar configurar servidores/instâncias, a AWS faz isso tudo sozinha, dando liberdade para que o desenvolvedor foque em desenvolver suas aplicações. É uma aplicação server less.

[[Outros Serviços AWS...]]
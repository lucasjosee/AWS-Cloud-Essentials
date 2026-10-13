
### Armazenamento em bloco
O armazenamento em bloco fornece volumes de armazenamento persistentes e de baixa latência em nível de bloco que se conectam a instâncias do EC2, como discos rígidos físicos. 

##### Armazenamento de instância do Amazon EC2
O armazenamento de bloco de um instância EC2 funciona como uma maquina normal. Porém, esses dados gravados durante o tempo que a instância esteve ligada não persistem. 

##### Amazon Elastic Block Store (EBS)
O Amazon EBS disponibiliza volumes de _armazenamento no nível de bloco persistentes_ para uso com instâncias do Amazon EC2. Os volumes do EBS funcionam como discos rígidos externos, oferecendo desempenho consistente e de baixa latência para workloads como bancos de dados e sistemas de arquivos.

O usuário é responsável por cuidar dos snapshots do EBS, com isso temos a ferramenta **Amazon Data Lifecycle Manager**.

### Armazenamento de objetos
O armazenamento de objetos é uma arquitetura de armazenamento de dados que gerencia dados como objetos em um espaço de endereço plano. Ele oferece escalabilidade ilimitada para que você possa armazenar grandes quantidades de dados não estruturados sem se preocupar com restrições de capacidade.

##### Amazon Simple Storage Service (S3)
O Amazon S3 é um serviço de armazenamento de objetos totalmente gerenciado e altamente disponível para armazenar e recuperar qualquer quantidade de dados como objetos. Ele oferece durabilidade de 99,999999999%, o que significa que seus dados estão altamente protegidos contra perdas e oferece atributos como versionamento, gerenciamento do ciclo de vida e várias classes de armazenamento para otimizar custos.

### Armazenamento de arquivos
Os serviços de armazenamento de arquivos da AWS fornecem sistemas de arquivos compartilhados acessíveis por meio de redes, para que vários usuários e aplicações possam acessar os mesmos dados simultaneamente.
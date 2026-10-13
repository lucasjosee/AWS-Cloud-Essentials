
### Armazenamento em bloco
O armazenamento em bloco fornece volumes de armazenamento persistentes e de baixa latência em nível de bloco que se conectam a instâncias do EC2, como discos rígidos físicos. Ideal para **SO, bancos de dados e apps** que alteram pequenas partes de arquivos com frequência.

##### Armazenamento de instância do Amazon EC2
O armazenamento de bloco de um instância EC2 funciona como uma maquina normal (disco físico no mesmo host). Porém, esses dados **não persistem**: são perdidos quando a instância é **parada ou encerrada**. Muito rápido, ideal para **dados temporários** (cache, buffers, arquivos temporários).

##### Amazon Elastic Block Store (EBS)
O Amazon EBS disponibiliza volumes de _armazenamento no nível de bloco persistentes_ para uso com instâncias do Amazon EC2. Os volumes do EBS funcionam como discos rígidos externos, oferecendo desempenho consistente e de baixa latência para workloads como bancos de dados e sistemas de arquivos. **Os dados continuam existindo mesmo se a instância for parada.**

- Um volume EBS fica em **uma única AZ** e só se conecta a instâncias **na mesma AZ**.
- **Snapshots**: backups do volume, **incrementais** (só salvam o que mudou desde o último) e armazenados no S3. Permitem recriar o volume em outra AZ.

O usuário é responsável por cuidar dos snapshots do EBS, com isso temos a ferramenta **Amazon Data Lifecycle Manager**, que automatiza a criação, retenção e exclusão de snapshots.

### Armazenamento de objetos
O armazenamento de objetos é uma arquitetura de armazenamento de dados que gerencia dados como objetos em um espaço de endereço plano. Ele oferece escalabilidade ilimitada para que você possa armazenar grandes quantidades de dados não estruturados sem se preocupar com restrições de capacidade. Cada objeto = **dados + metadados + chave** (identificador). Alterar um objeto = substituir o objeto inteiro.

##### Amazon Simple Storage Service (S3)
O Amazon S3 é um serviço de armazenamento de objetos totalmente gerenciado e altamente disponível para armazenar e recuperar qualquer quantidade de dados como objetos. Ele oferece durabilidade de **99,999999999% (11 noves)**, o que significa que seus dados estão altamente protegidos contra perdas.

- Objetos ficam em **buckets**; o nome do bucket é **único no mundo inteiro**.
- Tamanho máximo de um objeto: **5 TB**. Armazenamento total ilimitado.
- S3 é um serviço **regional** (dados replicados em no mínimo 3 AZs, exceto One Zone).
- **Versionamento**: guarda versões anteriores, protege contra exclusão/sobrescrita acidental.
- **Ciclo de vida (lifecycle)**: move objetos automaticamente para classes mais baratas ou os exclui após X dias.
- **Hospedagem de site estático** (HTML, CSS, imagens).
- Novos buckets são **privados por padrão** (Block Public Access ativado). O acesso é controlado por **políticas de bucket** e IAM.
- **Criptografia**: todo objeto novo é criptografado no servidor por padrão (SSE-S3). Também dá para usar chaves do **AWS KMS** (SSE-KMS).
- **Replicação**: copia objetos automaticamente para outro bucket, na mesma Região (**SRR**) ou em outra Região (**CRR**, usada para recuperação de desastres e conformidade). Exige versionamento ativado.
- **S3 Object Lock**: modelo **WORM** (*write once, read many*), que impede apagar ou alterar o objeto por um período. Palavra-chave: "dados não podem ser alterados/excluídos por exigência legal".
- **S3 Transfer Acceleration**: acelera **uploads de longa distância** para o bucket usando as **edge locations** do CloudFront.
- **Upload multipartes**: divide arquivos grandes em partes. Recomendado acima de 100 MB e obrigatório acima de 5 GB.

**Como o S3 cobra:** armazenamento (GB/mês, conforme a classe), requisições (GET/PUT), recuperação (classes IA/Glacier) e **transferência de dados para fora** da AWS. **Transferência de dados para dentro (upload) é gratuita.**

##### Classes de armazenamento do S3 (cai muito!)
Escolha pela **frequência de acesso** e **tempo de recuperação**:

| Classe | Quando usar | Recuperação |
| --- | --- | --- |
| **S3 Standard** | Dados acessados **com frequência**. | Milissegundos |
| **S3 Intelligent-Tiering** | Padrão de acesso **desconhecido ou que muda**. Move os objetos sozinho entre camadas. | Milissegundos |
| **S3 Standard-IA** | Acesso **infrequente**, mas precisa de acesso **imediato** quando pedido. | Milissegundos |
| **S3 One Zone-IA** | Igual ao Standard-IA, mas em **uma só AZ** (mais barato). Só para dados que podem ser **recriados**. | Milissegundos |
| **S3 Express One Zone** | Desempenho altíssimo, latência de um dígito de ms, uma só AZ. | Milissegundos |
| **S3 Glacier Instant Retrieval** | Arquivamento acessado raramente (~1x por trimestre), mas que precisa de acesso **imediato**. | Milissegundos |
| **S3 Glacier Flexible Retrieval** | Arquivamento; pode esperar. | Minutos a horas |
| **S3 Glacier Deep Archive** | **Mais barato de todos**. Retenção de longo prazo (ex.: **7 a 10 anos** por conformidade). | Até **12 horas** |

> **Pegadinhas:**
> - "Mais barato para arquivar por anos" → **Glacier Deep Archive**.
> - "Acesso imprevisível" → **Intelligent-Tiering**.
> - "Acesso raro, mas quando precisar tem que ser imediato" → **Standard-IA** ou **Glacier Instant Retrieval**.
> - "Dados que podem ser recriados facilmente" → **One Zone-IA**.
> - Classes IA e Glacier cobram **taxa de recuperação** e têm **tempo mínimo de armazenamento**.

### Armazenamento de arquivos
Os serviços de armazenamento de arquivos da AWS fornecem sistemas de arquivos compartilhados acessíveis por meio de redes, para que **vários usuários e instâncias possam acessar os mesmos dados simultaneamente**.

##### Amazon Elastic File System (EFS)
- Sistema de arquivos **compartilhado** para **Linux** (protocolo NFS).
- **Escala automaticamente** (cresce e diminui conforme os arquivos são adicionados/removidos), paga só pelo que usa.
- **Regional**: dados em várias AZs, e instâncias de **várias AZs** acessam ao mesmo tempo.

##### Amazon FSx
Sistemas de arquivos de terceiros totalmente gerenciados:
- **FSx for Windows File Server** → aplicações **Windows** (protocolo SMB).
- **FSx for Lustre** → **HPC** e machine learning (alto desempenho).
- **FSx for NetApp ONTAP** / **FSx for OpenZFS** → migrar sistemas NetApp/ZFS que já existem on-premises.

### Outros serviços de armazenamento
| Serviço | O que faz |
| --- | --- |
| **AWS Storage Gateway** | Armazenamento **híbrido**: conecta sistemas **on-premises** ao armazenamento da AWS (S3 etc.). |
| **AWS Backup** | Gerencia e automatiza backups de **vários serviços** (EBS, EFS, RDS, DynamoDB...) em um só lugar. |
| **AWS DataSync** | Transfere dados **online** (pela rede) de servidores on-premises (NFS/SMB) para S3, EFS ou FSx, de forma automatizada e agendada. |
| **AWS Elastic Disaster Recovery** | Replica servidores para a AWS para recuperá-los rapidamente em caso de desastre. |

##### Família AWS Snow (transferência física/offline)
Dispositivos físicos que a AWS **envia para você**. Você copia os dados e devolve o aparelho, e a AWS carrega os dados no S3. Usado quando há **muitos dados (TB/PB) e a internet é lenta ou cara**. Os dispositivos também processam dados no local (computação de borda).
- **Snowball Edge**: o dispositivo principal, com versões *Storage Optimized* e *Compute Optimized*.
- **Snowcone** (pequeno) e **Snowmobile** (caminhão, nível exabyte) **foram descontinuados em 2024**. Desde nov/2025 o Snowball Edge também **não aceita clientes novos**. A AWS agora recomenda o DataSync para transferência online.

> Mesmo descontinuada, a família Snow ainda está no guia do CLF-C02 e aparece em simulados. Saiba que **"migrar petabytes com internet ruim"** → **Snowball Edge**.

### Resumo: EBS x S3 x EFS x Instance Store
| | Instance Store | EBS | S3 | EFS |
| --- | --- | --- | --- | --- |
| Tipo | Bloco | Bloco | Objeto | Arquivo |
| Persistente? | **Não** | Sim | Sim | Sim |
| Escopo | Host da instância | **1 AZ** | **Região** | **Região (multi-AZ)** |
| Acesso | 1 instância | 1 instância* | Qualquer lugar via HTTP/API | **Várias instâncias ao mesmo tempo** |
| Uso típico | Dados temporários | Disco de boot, banco de dados | Backups, mídia, sites estáticos, data lakes | Pasta compartilhada entre servidores Linux |

\* EBS Multi-Attach existe, mas só para alguns tipos de volume e dentro da mesma AZ.

> **Pegadinhas:**
> - "Dados precisam sobreviver ao encerramento da instância" → **EBS ou S3**, nunca instance store.
> - "Vários servidores Linux acessando os mesmos arquivos" → **EFS**.
> - "Arquivo compartilhado para Windows" → **FSx for Windows**.
> - "Editar um pedaço pequeno de um arquivo grande com frequência" → **bloco (EBS)**, não S3 (no S3 o objeto inteiro é reescrito).
> - "Arquivos não podem ser apagados/alterados por X anos (conformidade)" → **S3 Object Lock**.
> - "Cópia dos dados em outra Região para recuperação de desastres" → **Cross-Region Replication**.
> - "Uploads lentos de usuários do mundo todo para o S3" → **S3 Transfer Acceleration**.
> - "Migrar dados on-premises pela rede, de forma automatizada" → **DataSync**. "Sem internet boa / volume enorme" → **Snowball Edge**.
> - "App on-premises usando armazenamento na nuvem (híbrido)" → **Storage Gateway**.
> - Proteger dados no S3 (políticas de bucket, criptografia, quem tem acesso) é **responsabilidade do cliente**.

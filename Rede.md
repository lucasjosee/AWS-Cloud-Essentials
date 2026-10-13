#### Amazon Virtual Private Cloud(Amazon VPC)
A **Amazon VPC (Virtual Private Cloud)** é a sua **rede privada dentro da AWS**. É um pedaço isolado da nuvem onde seus recursos (instâncias EC2, bancos RDS, containers) vivem, e onde você decide quem entra, quem sai e quem conversa com quem.

- Uma VPC pertence a **uma Região** e pode abranger todas as AZs dela.
- Uma **sub-rede** fica em **uma única AZ**.
- Toda conta já vem com uma **VPC padrão** em cada Região, com uma sub-rede pública em cada AZ.

![[Pasted image 20261008192748.png]]

Tudo que está dentro da VPC consegue se comunicar entre si (respeitando as ACLs e grupos de segurança), independente se está em outra AZ ou se a sub-rede é publica ou privada.

**Sub-rede pública x privada:**
- **Pública**: tem rota para o gateway de internet. Ex.: servidores web.
- **Privada**: sem rota direta para a internet. Ex.: **bancos de dados**.

#### Gateways
- **Gateway de internet**: conexão entre a VPC e a internet. Sem ele, nada da internet entra na VPC. Você pode pensar como a porta que os clientes usam para entrar na cafeteria.
- **NAT gateway**: permite que recursos de uma sub-rede **privada** acessem a internet (ex.: baixar atualizações) **sem** que a internet consiga iniciar conexão com eles.
- **Gateway privado virtual (Virtual Private Gateway)**: ponto de entrada na VPC para conexões **VPN** vindas da sua rede on-premises.

##### Maneiras de se conectar na nuvem AWS:

| Serviço | O que é | Palavra-chave na prova |
| --- | --- | --- |
| **AWS Client VPN** | VPN para **usuários individuais** (profissionais remotos) acessarem a AWS/rede da empresa. | "funcionários remotos" |
| **AWS Site-to-Site VPN** | Túnel **criptografado pela internet pública** entre o data center/filial e a VPC. | "rápido de configurar", "baixo custo", "criptografado pela internet" |
| **AWS Direct Connect** | Linha **física dedicada e privada** entre seu data center e a AWS. **Não passa pela internet.** | "conexão dedicada", "desempenho consistente", "alta largura de banda", "reduzir custos de transferência" |
| **AWS PrivateLink** | Acessar serviços da AWS ou de terceiros **de forma privada**, sem expor o tráfego à internet. | "sem passar pela internet pública" |

> **Pegadinha:** Direct Connect **não é criptografado por padrão** e leva semanas para ficar pronto. É comum usar uma Site-to-Site VPN como backup do Direct Connect.

#### Tráfego de rede em uma VPC
Todos os dados que entram e saem de uma VPC são carregados em **pacotes**. Um pacote, é uma unidade de dados enviada pela internet ou por uma rede. Ele entra pelo gateway, e antes de entrar em uma sub-rede e chegar ao recurso, varias verificações de segurança são feitas.

![[Pasted image 20261010001545.png]]
##### ACLs de Rede
Firewall virtual que controla o tráfego **no nível da sub-rede**.

Cada conta AWS tem uma ACL de rede-padrão, que **permite todo o tráfego** de entrada e saída, mas é possível modificá-la ao adicionar suas regras. ACLs personalizadas **negam tudo** até você adicionar regras. Aceitam regras de **permitir e negar** (útil para bloquear um IP específico). São **stateless**, não se lembram de quem entrou e nem de quem saiu, por isso que fazem a verificação em ambos os sentidos.

##### Grupos de Segurança
Firewall virtual que controla o tráfego **no nível do recurso** (ex.: instância EC2) dentro da sub-rede.

Por padrão, um grupo de segurança **nega todo o tráfego de entrada e permite todo o tráfego de saída**. Só aceita regras de **permitir** (não dá para criar regra de negar). É **stateful**, então lembra quem entra e libera a resposta automaticamente.

PS: Tive um problema de grupo de segurança ao lançar uma instância, não estava na lista de permissões a conexão HTTP, tive que adicionar e só assim consegui me conectar.

> **Pegadinhas:**
> - "Bloquear um IP específico" → **ACL de rede** (grupo de segurança não tem regra de negar).
> - Configurar grupos de segurança e ACLs é **responsabilidade do cliente**.

#### DNS e entrega de conteúdo
| Serviço | O que faz |
| --- | --- |
| **Amazon Route 53** | Serviço de **DNS** (traduz nome → IP), registra domínios e faz roteamento (por latência, geolocalização, failover, ponderado). |
| **Amazon CloudFront** | **CDN**: guarda cache do conteúdo nas **edge locations** perto do usuário → menor latência. |

> **Pegadinha:** Route 53 decide **para onde** o usuário vai. CloudFront entrega o conteúdo **mais rápido**. "Redirecionar para site reserva se o principal cair" → Route 53 (failover).

#### Resumo rápido
| Cenário | Resposta |
| --- | --- |
| Rede isolada na AWS | VPC |
| Banco de dados fora do alcance da internet | Sub-rede privada |
| Sub-rede privada precisa baixar atualizações | NAT gateway |
| Firewall da instância | Grupo de segurança |
| Firewall da sub-rede | ACL de rede |
| Link rápido e barato, criptografado pela internet | Site-to-Site VPN |
| Link dedicado, consistente, sem internet | Direct Connect |
| Nome de domínio / DNS | Route 53 |
| Cache de conteúdo perto do usuário | CloudFront |

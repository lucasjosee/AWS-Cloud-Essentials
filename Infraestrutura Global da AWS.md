#### Regiões
Uma **Região** é uma área geográfica (ex.: `sa-east-1` São Paulo) com várias Zonas de Disponibilidade. As Regiões são **isoladas umas das outras**: os dados **não saem de uma Região** sem você autorizar.

#### Principais considerações ao escolher Regiões
![[Pasted image 20261008170410.png]]
1. **Conformidade**: leis e regras de governança de dados (ex.: dados precisam ficar no país). Costuma ser o fator decisivo.
2. **Proximidade**: Região perto dos clientes = menor latência.
3. **Serviços disponíveis**: nem todo serviço/recurso novo existe em todas as Regiões.
4. **Preços**: o mesmo serviço custa diferente em cada Região (ex.: São Paulo costuma ser mais cara).

#### Zonas de Disponibilidade (AZs)
- Uma **AZ** é **um ou mais data centers** dentro de uma Região, com energia, rede e refrigeração próprias.
- AZs ficam a dezenas de km umas das outras (isoladas contra desastres) mas conectadas com baixa latência.
- Cada Região tem **no mínimo 3 AZs**.
- **Boa prática**: rodar a aplicação em **pelo menos 2 AZs** → alta disponibilidade. Se uma AZ cair, a outra continua.

#### Locais de borda (Edge Locations)
- Locais espalhados pelo mundo (**muito mais numerosos que Regiões**) usados para entregar conteúdo com baixa latência perto do usuário.
- Usados pelo **Amazon CloudFront** (CDN: guarda cache de conteúdo perto do usuário) e pelo **Amazon Route 53** (DNS).

#### Outras extensões da infraestrutura
| Serviço | Para quê |
| --- | --- |
| **AWS Outposts** | Infraestrutura AWS **dentro do seu data center** (nuvem híbrida). |
| **Local Zones** | Serviços AWS mais perto de grandes cidades, para latência de milissegundos. |
| **Wavelength** | Serviços AWS dentro de redes **5G** de operadoras (apps móveis de latência ultrabaixa). |

#### Formas de interagir com a AWS
Tudo na AWS é uma **chamada de API**. Você pode chamar essas APIs por:
- **Console de Gerenciamento da AWS**: interface web (visual).
- **AWS CLI**: linha de comando, permite automatizar com scripts.
- **SDKs**: bibliotecas para usar a AWS dentro do código (Python, Java...).

#### Infraestrutura como Código (IaC)
- **AWS CloudFormation**: cria recursos a partir de **templates JSON/YAML**, de forma repetível e em várias contas/Regiões. Serviço gratuito (paga só os recursos criados).

> **Pegadinhas:**
> - Alta disponibilidade → **várias AZs**. Recuperação de desastre / tolerar falha de uma região inteira → **várias Regiões**.
> - Entregar conteúdo com baixa latência para o mundo todo → **CloudFront + edge locations** (não "criar mais Regiões").
> - O cliente **escolhe a Região**; a AWS nunca move dados entre Regiões sozinha.

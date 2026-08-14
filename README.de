# AZ-900 — Conceitos Fundamentais de Nuvem

**Trilha:** Microsoft Learn — AZ-900T00 (Microsoft Azure Fundamentals)
**Módulo:** Descrever conceitos de nuvem
**Data:** Agosto/2026
**Status:** Revisão concluída (módulo já havia sido cursado anteriormente, em jan/2026)

## Contexto

Este módulo é o ponto de partida da trilha AZ-900, certificação introdutória do Azure. Ele cobre os fundamentos econômicos e arquiteturais da computação em nuvem — a base conceitual sobre a qual todo o resto da certificação (serviços, governança, segurança) se apoia.

## Tópicos revisados

### 1. CapEx vs OpEx

| | CapEx (Capital Expenditure) | OpEx (Operational Expenditure) |
|---|---|---|
| Modelo | Investimento antecipado em ativos físicos | Pagamento recorrente pelo uso |
| Exemplo | Compra de switch core, servidores, licenças perpétuas | Assinatura de VM/serviço em nuvem, pago por hora/mês |
| Risco | Superdimensionamento ou subdimensionamento gera desperdício ou gargalo | Escala conforme a demanda real |

A computação em nuvem é um modelo OpEx: em vez de comprar hardware, a empresa paga apenas pelos recursos efetivamente consumidos, eliminando o investimento inicial pesado e o risco de dimensionamento incorreto.

**Aplicação prática discutida:** um projeto de migração de rede com compra de switch core e instalação física em sala de servidor é um exemplo claro de CapEx — o equipamento vira patrimônio da empresa e deprecia independentemente do nível de uso. O equivalente em nuvem (ex: Azure Virtual Network) seria OpEx, sem hardware próprio envolvido.

### 2. Modelos de serviço — IaaS, PaaS e SaaS

Os três modelos definem a divisão de responsabilidade entre cliente e provedor de nuvem:

- **IaaS (Infrastructure as a Service):** o provedor entrega a infraestrutura bruta (servidor virtual); o cliente gerencia sistema operacional, patches, aplicações e configurações.
  *Exemplo:* Azure Virtual Machines.

- **PaaS (Platform as a Service):** o provedor gerencia o sistema operacional e a infraestrutura subjacente; o cliente sobe apenas sua aplicação/código.
  *Exemplo:* Azure App Service.

- **SaaS (Software as a Service):** o provedor entrega o software pronto para uso; o cliente apenas consome a funcionalidade.
  *Exemplo:* Microsoft 365.

**Exercício de fixação — Active Directory local vs Entra ID:**

Foi discutida a diferença entre migrar o Active Directory tradicional para uma VM no Azure versus adotar o Entra ID:

- **AD DS em VM no Azure → IaaS.** A empresa continua responsável por gerenciar o sistema operacional, aplicar patches, configurar o AD DS e manter GPOs — a Microsoft entrega apenas a máquina virtual.
- **Entra ID → SaaS.** É um serviço de identidade totalmente gerenciado pela Microsoft; não há sistema operacional ou servidor para administrar — a configuração se limita a usuários, grupos e políticas de acesso via portal.

Essa distinção é frequentemente confundida no mercado: "levar o AD para a nuvem" (IaaS) e "adotar o Entra ID" (SaaS) são abordagens diferentes, que podem inclusive coexistir em um cenário híbrido (VMs com AD DS sincronizando com o Entra ID via Entra Connect).

### 3. Modelos de implantação — pública, privada e híbrida

- **Nuvem pública:** infraestrutura compartilhada entre múltiplos clientes, fornecida por provedores como Azure ou AWS.
- **Nuvem privada:** infraestrutura de nuvem dedicada a uma única organização, geralmente hospedada no próprio data center da empresa.
- **Nuvem híbrida:** combinação de ambiente on-premises com nuvem pública, operando de forma integrada — não apenas coexistindo, mas trocando dados entre si.

**Aplicação prática discutida:** um cenário híbrido relevante para o meu contexto atual seria manter os DCs (Active Directory) on-premises e sincronizar usuários e grupos com o Entra ID via Entra Connect, permitindo autenticação única (mesma senha) tanto para recursos locais (arquivos, impressoras) quanto para serviços em nuvem (Microsoft 365, aplicações Azure). Esse é o modelo mais comum encontrado no mercado, já que a maioria das empresas não migra a infraestrutura inteira para a nuvem de uma vez.

## Conclusão

Módulo de conceitos de nuvem revisado e consolidado. Os três pilares (CapEx/OpEx, modelos de serviço, modelos de implantação) formam a base conceitual da certificação AZ-900 e conectam diretamente com a experiência prévia em Active Directory on-premises — especialmente no entendimento do cenário híbrido via Entra Connect, que representa um diferencial real para atuação em infraestrutura com foco em nuvem.

**Próximo passo:** avançar para o módulo de arquitetura e serviços principais do Azure (trilha AZ-900T00).

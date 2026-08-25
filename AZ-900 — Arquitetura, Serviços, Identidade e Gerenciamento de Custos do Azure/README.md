# AZ-900 — Arquitetura, Serviços, Identidade e Gerenciamento de Custos do Azure

**Trilha:** Microsoft Learn — AZ-900T00 (Microsoft Azure Fundamentals)
**Módulos:** Componentes arquitetônicos, Computação, Rede, Armazenamento, Identidade/Segurança, Gerenciamento de Custos
**Data:** Agosto/2026
**Status:** Trilha AZ-900 completa — todos os módulos concluídos com avaliação aprovada

## Contexto

Esta sessão fecha a trilha completa do AZ-900, cobrindo os módulos realizados de forma independente no Microsoft Learn e revisados em sessão de fixação. É a continuação direta do relatório anterior sobre conceitos fundamentais de nuvem (CapEx/OpEx, modelos de serviço, modelos de implantação).

Com o AZ-900 concluído, o próximo passo é avançar para o AZ-104 (Azure Administrator), foco real do meu objetivo de atuação em administração de cloud.

## Tópicos revisados

### 1. Componentes arquitetônicos — Resource Groups

Um **Grupo de Recursos (Resource Group)** é um contêiner lógico que agrupa recursos do Azure que compartilham o mesmo ciclo de vida — não apenas recursos "parecidos", mas recursos que pertencem ao mesmo projeto e que devem ser gerenciados, monitorados e eventualmente removidos em conjunto.

**Vantagens práticas:**
- Remoção em bloco: deletar o Resource Group remove todos os recursos associados, evitando recursos órfãos gerando custo.
- Aplicação de permissões (RBAC) e políticas no nível do grupo inteiro.
- Visibilidade de custo por projeto, já que a fatura pode ser filtrada por Resource Group.

**Boa prática discutida:** recursos temporários ou de teste devem ficar em um Resource Group próprio, isolado do ambiente de produção — permitindo testar livremente e depois remover tudo sem risco de impactar recursos em uso.

### 2. Computação — VMs, Contêineres e App Service

O Azure oferece três abordagens principais para executar aplicações, cada uma com um nível diferente de responsabilidade de gerenciamento:

- **Máquinas Virtuais (IaaS):** controle total sobre o sistema operacional — indicada para aplicações legadas ou que exigem configuração customizada de SO.
- **Contêineres:** mais leves e portáveis que uma VM, garantem consistência entre ambientes (dev, teste, produção).
- **App Service (PaaS):** entrega a infraestrutura e o ambiente de execução prontos — a aplicação é publicada diretamente, sem necessidade de gerenciar SO, patches ou servidor web.

**Aplicação prática discutida:** para uma aplicação web simples, sem necessidade de customização de infraestrutura, o App Service é a escolha mais eficiente — elimina a responsabilidade de gerenciamento de sistema operacional.

### 3. Rede — Virtual Network (VNet) e segmentação

Uma **Virtual Network (VNet)** é o equivalente, no Azure, a uma rede isolada onde os recursos se comunicam entre si — conceito análogo à segmentação de rede física já praticada com sub-redes e VLANs.

**Motivos para dividir uma VNet em múltiplas sub-redes:**
- **Segurança:** cada sub-rede pode ter seu próprio NSG (Network Security Group), permitindo isolar camadas (ex: banco de dados só aceita tráfego da camada de aplicação, nunca exposto diretamente à internet).
- **Organização e controle de impacto:** separar por camada de aplicação (web, aplicação, dados) facilita monitoramento e limita o alcance de um problema a um segmento específico, sem afetar a rede inteira.

### 4. Armazenamento — Blob, Files e Disk

O Azure Storage possui três tipos principais, cada um adequado a um cenário distinto:

- **Blob:** armazenamento de dados não estruturados (imagens, vídeos, backups), tipicamente acessado via código/API.
- **Files:** compartilhamento de arquivos via protocolo SMB — pode ser mapeado como uma unidade de rede tradicional, sem alterar a forma como os usuários já acessam arquivos.
- **Disk:** discos utilizados exclusivamente por VMs, acessados apenas através da própria máquina virtual.

**Aplicação prática discutida:** para migrar uma pasta compartilhada de rede da empresa para o Azure sem alterar a experiência dos usuários, o Azure Files é a opção correta, por preservar o acesso via caminho de rede (SMB) que os usuários já utilizam.

### 5. Identidade e Segurança — RBAC

**RBAC (Role-Based Access Control)** é o mecanismo do Azure para controle de acesso baseado em papéis — conceito equivalente à delegação de permissões em OUs no Active Directory, mas aplicável tanto a usuários individuais quanto a grupos, e em diferentes escopos (assinatura, Resource Group ou recurso único).

**Por que usar RBAC em vez de conceder Administrador Global a todos os usuários:**
- Aplica o princípio do menor privilégio — cada pessoa tem acesso apenas ao necessário para sua função.
- Reduz o impacto de uma conta comprometida: o dano fica limitado ao escopo do papel atribuído, em vez de expor o ambiente inteiro.
- Permite granularidade cirúrgica (ex: acesso restrito apenas a gerenciamento de VMs, sem acesso a storage ou rede).

### 6. Gerenciamento de Custos

Diferente do modelo CapEx — onde o gasto é fixo e definido no momento da compra —, o modelo OpEx da nuvem gera custos variáveis e contínuos: cada recurso ativo continua sendo cobrado enquanto estiver em execução, mesmo que esquecido ou subutilizado.

**Por que o monitoramento de custos é mais crítico na nuvem:** ao contrário do CapEx, que tem um teto de gasto natural definido na compra, o modelo OpEx não possui esse limite automático — um recurso de teste esquecido ou mal dimensionado pode gerar custo crescente sem que ninguém perceba. Ferramentas como o Azure Cost Management, orçamentos (budgets) e alertas de custo são práticas essenciais para evitar esse cenário, não apenas para confirmar que os recursos pagos estão sendo utilizados.

## Conclusão

Com esta sessão, a trilha AZ-900 foi concluída em sua totalidade: conceitos de nuvem, componentes arquitetônicos, computação, rede, armazenamento, identidade/segurança e gerenciamento de custos — todos os módulos com avaliação aprovada no Microsoft Learn e conhecimento consolidado através de perguntas de fixação aplicadas na prática, conectando cada conceito à experiência prévia em Active Directory e infraestrutura on-premises.

**Próximo passo:** iniciar a trilha AZ-104 (Azure Administrator), com foco em administração prática de identidade/governança, armazenamento, computação, rede virtual e monitoramento — habilidades diretamente alinhadas ao objetivo de atuação em administração de cloud.

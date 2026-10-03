# Relatório de Exercício — AZ-104 Domínio 3

## Implantar e Gerenciar Recursos de Computação do Azure

**Autor:** Dalgeni Burak **Domínio do exame:** Deploy and manage Azure compute resources (\~20-25%)

---

## 1. Máquina Virtual (VM)

| Recurso | Detalhe |
| --- | --- |
| VM | `vm-teste-dalgeni`, Windows Server 2025 Datacenter |
| Tamanho | Standard_D2as_v4 (2 vCPUs, 8 GiB RAM) |
| Região/Zona | East US, Availability Zone 1 |
| Resource Group | `RG-TesteVM` |
| Acesso | RDP (porta 3389), conexão validada com sucesso |
| Auto-shutdown | Configurado |

**Recursos de dependência criados automaticamente:** IP público, Network Security Group, Network Interface, Disco gerenciado (OS Disk), Virtual Network — confirmando que toda VM no Azure vem acompanhada de um conjunto mínimo de dependências de rede e armazenamento.

**Conceitos praticados:**

- RDP exige liberação explícita de porta via NSG — nada é acessível por padrão, só o que é solicitado na criação.
- Status "Stopped (deallocated)" interrompe a cobrança por computação (diferente de apenas desligar o SO pelo Windows), liberando o hardware reservado.
- Troca de tamanho de VM exige a máquina desligada, pois pode envolver realocação para hardware físico diferente.
- Cota de assinatura (quota) pode bloquear certas famílias de VM (ex: série B indisponível no trial) independente da região — visível nas categorias "Insufficient quota - family/regional limit" na tela de seleção de tamanho.

## 2. Discos e Availability Zones

**Conceitos praticados:**

- Tipos de disco gerenciado (Standard HDD, Standard SSD, Premium SSD, Ultra Disk) têm trade-off de custo por GB vs performance (IOPS/latência) — equivalente ao conceito já visto em tiers de Storage.
- Availability Zone define apenas a localização física da VM — uma única VM numa zona não garante alta disponibilidade sozinha. Alta disponibilidade real exige múltiplas VMs em zonas diferentes + Load Balancer com health check, redirecionando tráfego automaticamente em caso de falha de uma zona.

## 3. Azure App Service

| Recurso | Detalhe |
| --- | --- |
| Web App | `app-teste-dalgeni` |
| App Service Plan | Basic B1 (tier Free F1 bloqueado por cota na assinatura trial) |
| Runtime | .NET 10 (LTS), Windows |
| Resource Group | `RG-TesteApp` |
| Resultado | Aplicação ativa e acessível via `azurewebsites.net`, aguardando conteúdo |

**Conceitos praticados:**

- App Service é PaaS (Platform as a Service): sem gestão de SO/patch, diferente de VM (IaaS), onde toda a infraestrutura é responsabilidade do administrador.
- Múltiplas aplicações podem compartilhar um único App Service Plan para otimizar custo, desde que a soma do uso caiba na capacidade contratada; planos separados fazem sentido quando uma app precisa de isolamento de performance.

## 4. Azure Container Instances

| Recurso | Detalhe |
| --- | --- |
| Container | `container-teste-dalgeni` |
| Imagem | mcr.microsoft.com/azuredocs/aci-helloworld (quickstart) |
| Resource Group | `RG-TesteContainer` |
| Resultado | Container rodando, IP público, acesso validado via navegador |

**Conceito praticado:** forma mais simples de rodar um container isolado sob demanda, sem necessidade de VM dedicada nem cluster Kubernetes completo.

## Domínio 3 — Status final

✅ Máquina Virtual (criação, conexão RDP, shutdown/deallocation) ✅ Tamanhos de VM e discos gerenciados (conceitual + prático) ✅ Availability Zones (conceitual) ✅ Azure App Service (PaaS) ✅ Azure Container Instances

**Domínio 3 do AZ-104 concluído na prática.** Próximo passo do roteiro: Domínio 4 — Configurar e gerenciar redes virtuais.

## Nota de limpeza de recursos

VM, App Service e Container Instances seguem ativos nos respectivos Resource Groups. Recomenda-se desligar/deletar os recursos não utilizados antes da expiração dos créditos do trial, para evitar consumo desnecessário.

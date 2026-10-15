# Relatório de Exercício — AZ-104 Domínio 5
## Monitoramento e Backup: Alertas, Activity Log, Log Analytics e Recuperação

**Autor:** Dalgeni Burak
**Domínio do exame:** Monitor and maintain Azure resources (~10-15%)

---

## Objetivo

Praticar a detecção e a investigação de eventos no Azure: criar um alerta sobre uma mudança sensível de rede, enviar o Activity Log para um Log Analytics workspace e consultá-lo com KQL. A parte de backup foi praticada no Domínio 2 e complementada aqui com a teoria de restauração e Site Recovery.

## 1. Alerta de Activity Log

| Recurso | Detalhe |
|---|---|
| Resource group | `RG-TesteMonitor` |
| Action group | `ag-email-dalgeni` (notificação por e-mail) |
| Alert rule | `alerta-delete-nsg`, escopo na assinatura, sinal "Delete Network Security Group" (categoria Administrative) |

**Resultado:** a regra **disparou** ao apagar um NSG de teste, confirmado pelo evento "Activity Log Alert activated" no próprio Activity Log. O e-mail de teste do action group chegou, mas a notificação do alerta real não foi recebida, e, ao refazer o action group, o e-mail de verificação também não chegou. O reteste da entrega por e-mail ficou adiado, e o fluxo foi consolidado na teoria.

**Conceitos praticados:**
- A regra define *quando* alertar, o Action Group define *quem* é avisado e *como*, e o mesmo grupo pode ser reaproveitado por várias regras.
- O destinatário precisa confirmar o e-mail antes de receber notificações.
- O botão *Test* do action group envia valores de exemplo e prova a entrega, mas não prova que a regra existe.
- Um alerta tem duas dimensões separadas: a *monitor condition* (Fired ou Resolved), mantida pelo sistema, e o *alert state* (New, Acknowledged ou Closed), mantido por quem trata o alerta.
- Tipos de alerta: *metric*, *log search* e *Activity log*.

## 2. Log Analytics e KQL

| Recurso | Detalhe |
|---|---|
| Workspace | `law-teste-dalgeni`, `RG-TesteMonitor`, East US |
| Diagnostic setting | `ds-activitylog-law`, categoria Administrative enviada ao workspace |

Consulta executada em KQL mode:

```kusto
AzureActivity
| where TimeGenerated > ago(1h)
| project TimeGenerated, Caller, OperationNameValue, ActivityStatusValue, ResourceGroup
| order by TimeGenerated desc
```

**Resultado:** 8 eventos com o usuário em `Caller`. Seis `RESOURCEGROUPS/WRITE` no resource group de teste (pares Start/Success da criação) e dois `DIAGNOSTICSETTINGS/WRITE`, a própria criação da diagnostic setting.

**Conceitos praticados:**
- O portal guarda o Activity Log por 90 dias. Enviá-lo ao workspace permite controlar a retenção, consultar com KQL, cruzar com outras tabelas e criar alertas baseados em consulta.
- Uma diagnostic setting não é retroativa: só envia os eventos posteriores à sua criação.
- `OperationNameValue` segue o formato *provedor / tipo de recurso / ação*, e `ActivityStatusValue` mostra as fases (Start, Success).
- Os horários vêm em UTC, o que exige conversão de fuso ao montar uma linha do tempo.

## 3. Backup e recuperação

**Praticado (Domínio 2):** Backup vault, política com camadas operacional (7 dias) e vaulted (90 dias), e a atribuição de RBAC à managed identity do cofre. Os jobs agendados rodaram (3 concluídos), e a falha registrada veio depois de a fonte de dados ser apagada.

**Estudado na teoria:**
- O Recovery Services vault cobre VMs e Azure Files, e o Backup vault cobre Blobs, discos e bancos.
- Uma VM pode ser restaurada criando uma VM nova, restaurando só os discos, substituindo a existente ou recuperando arquivos avulsos (file recovery).
- O soft delete protege contra a exclusão acidental ou maliciosa de backups.
- Backup recupera dados de um ponto no tempo, enquanto o Site Recovery replica a VM para outra região e permite failover para manter a carga de trabalho no ar, com test failover sem afetar a produção.
- Network Watcher: IP flow verify (diz se um NSG permite ou bloqueia um pacote e qual regra decidiu), Next hop e Connection troubleshoot.
- Azure Advisor: recomendações em cinco categorias (confiabilidade, segurança, custo, excelência operacional e desempenho).

## Observações práticas

- O nome "Workspaces" existe em dois serviços: o do Azure Virtual Desktop e o **Log Analytics workspaces**. Conferir o tipo abaixo do título.
- A busca por sinal de alerta exige o nome completo (por exemplo "network security group"), e uma busca parcial pode trazer sinais de outros serviços.
- Ao criar uma regra de alerta, conferir na aba Details qual resource group foi selecionado, porque o assistente pode escolher outro.
- O Logs do workspace tem um assistente de IA ligado por padrão (chave Agent). Para escrever e rodar KQL, usar o KQL mode.

## Domínio 5 — Status final

✅ Activity Log e regra de alerta (a regra disparou)
⚠️ Entrega do alerta por e-mail: pendente de reteste
✅ Log Analytics workspace, diagnostic setting e consulta KQL
✅ Backup (prática no Domínio 2) e teoria de restauração e Site Recovery
✅ Network Watcher e Azure Advisor (teoria)

**Os cinco domínios do AZ-104 foram percorridos.** Próxima etapa: diagnóstico com o practice assessment oficial e reforço das lacunas.

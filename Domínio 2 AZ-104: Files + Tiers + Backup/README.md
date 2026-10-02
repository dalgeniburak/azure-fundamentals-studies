# Relatório de Exercício — AZ-104 Domínio 2 (parte 2)

## Armazenamento: Azure Files, Tiers de Acesso e Azure Backup

**Autor:** Dalgeni Burak **Domínio do exame:** Implement and manage storage (\~15-20%)

---

## 1. Azure Files

| Recurso | Detalhe |
| --- | --- |
| File share | `fileshare-teste`, quota 10 GiB |
| Protocolo | SMB (classic file share) |
| Montagem | Drive `Z:` montado via PowerShell, usando Access Key |

**Teste realizado:** montagem bem-sucedida como unidade de rede local, análoga ao drive-map via GPO em AD local.

**Conceitos praticados:**

- Blob Storage é orientado a API/aplicação; Azure Files é orientado a acesso tipo "pasta de rede" (SMB).
- Montagem via Access Key dá acesso total e permanente à Storage Account inteira — adequado só para teste. Em produção, o recomendado é Identity-based access (Entra ID + RBAC), seguindo o mesmo princípio de menor privilégio já visto em RBAC geral.
- **Nota de segurança:** a Access Key apareceu em texto puro num print durante o exercício — reforçado o cuidado de nunca compartilhar esse tipo de print publicamente (GitHub, LinkedIn), e de regenerar a chave se isso ocorrer.

## 2. Tiers de acesso na prática

| Ação | Resultado |
| --- | --- |
| Mudança Hot → Cool | Aplicada sem erro, acesso ao blob mantido normalmente |
| Mudança Cool → Archive | Aplicada sem erro |
| Tentativa de acesso ao blob em Archive (via SAS Token válido) | Erro `BlobArchived` — "This operation is not permitted on an archived blob" |

**Conceito praticado:** blobs em Archive exigem reidratação (processo que pode levar horas) antes de qualquer leitura, mesmo com permissão de acesso válida — trade-off de custo muito baixo por armazenamento vs indisponibilidade imediata.

**Nota técnica adicional:** confirmado na prática que o SAS Token deve ser gerado a partir do blob específico, não do container — um token gerado no escopo do container não autentica corretamente uma URL apontando para um blob individual (erro `AuthenticationFailed — Signature did not match`).

## 3. Azure Backup

| Recurso | Detalhe |
| --- | --- |
| Backup vault | `vault-teste-backup`, East US, redundância LRS |
| Tipo de proteção | Azure Blobs (Azure Storage) |
| Política | Operational backup: retenção 7 dias / Vaulted backup: diário às 05:00 UTC, retenção 90 dias |
| Datasource protegido | Storage Account `sttestedalgeni` |

**Conceitos praticados:**

- Existem dois tipos de vault no Azure: **Backup vault** (suporta Blobs, Discos, bancos de dados, Kubernetes) e **Recovery Services vault** (único que suporta Azure Files e VMs) — escolha incorreta do tipo de vault bloqueia o datasource desejado.
- Backup funciona em camadas: operacional (recuperação rápida, retenção curta) + vaulted (recuperação mais lenta, retenção longa) — estratégia equivalente a combinar recuperação rápida para erros recentes com recuperação de longo prazo para cenários mais sérios.
- O Backup Vault usa uma **Managed Identity** própria para acessar os recursos protegidos, e essa identidade precisa de atribuição RBAC explícita (ex: "Storage Account Backup Contributor") antes de conseguir operar — o mesmo princípio de RBAC já visto se aplica também a identidades de serviço, não só usuários humanos.
- Security level "Poor" do vault refletiu Immutability e Soft Delete desabilitados na criação — indicador útil para avaliar postura de segurança de um cofre de backup.

## Domínio 2 — Status final

✅ Storage Account, Blob Storage, SAS Token ✅ Azure Files (compartilhamento SMB) ✅ Tiers de acesso (Hot/Cool/Archive) na prática ✅ Azure Backup (vault, política, RBAC de identidade gerenciada)

**Domínio 2 do AZ-104 concluído na prática.** Próximo passo do roteiro: Domínio 3 — Implantar e gerenciar recursos de computação do Azure.

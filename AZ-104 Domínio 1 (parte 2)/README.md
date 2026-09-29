# Relatório de Exercício — AZ-104 Domínio 1 (parte 2)

## Governança: Azure Policy, Management Groups e Cost Management

**Autor:** Dalgeni Burak **Domínio do exame:** Manage Azure identities and governance (\~20-25%)

---

## Objetivo

Fechar o Domínio 1 do AZ-104 praticando os mecanismos de governança complementares ao RBAC: controle sobre *o que* pode ser feito (Policy), organização hierárquica de múltiplas assinaturas (Management Groups) e monitoramento de custo (Cost Management).

## 1. Azure Policy

| Item | Detalhe |
| --- | --- |
| Política usada | "Locais permitidos" (built-in da Microsoft) |
| Escopo da atribuição | `RG-TesteRBAC` |
| Parâmetro | Apenas região East US permitida |
| Efeito | Deny |

**Teste realizado:** tentativa de criar uma Storage Account na região Brazil South dentro do `RG-TesteRBAC`. O Azure bloqueou já na etapa de validação (antes do clique em "Criar"), exibindo o erro "Locais permitidos" no campo Região.

**Conceitos fixados:**

- RBAC controla *quem* pode agir; Policy controla *o que* é permitido acontecer, independente da permissão do usuário.
- Preferir políticas built-in a customizadas sempre que cobrirem o caso — manutenção e confiabilidade ficam por conta da Microsoft.
- Efeitos de Policy: **Deny** (bloqueia), **Audit** (só registra, usado em rollout gradual ou compliance sem autoridade de bloqueio) e **Append** (corrige automaticamente sem travar).

## 2. Management Groups

Estrutura observada na hierarquia do tenant:

```
Tenant Root Group
    ├── Azure subscription 1
    └── MG-TesteEmpresa (grupo de teste, criado vazio)
```

**Conceitos fixados:**

- Hierarquia completa do Azure: Management Group → Assinatura → Resource Group → Recurso, com herança automática em cada nível.
- Toda assinatura já nasce dentro do Tenant Root Group por padrão — por isso a herança de Policy/RBAC já existia mesmo sem Management Groups customizados terem sido criados antes.
- Uso prático: aplicar a mesma Policy/RBAC de uma vez a múltiplas assinaturas (ex: garantir que nenhuma assinatura de uma empresa tenha recursos fora do Brasil), sem repetir configuração por assinatura.

## 3. Cost Management

| Item | Detalhe |
| --- | --- |
| Nome | `Budget-TrialOutubro` |
| Escopo | Assinatura inteira |
| Valor do orçamento | R$ 1.000,00 |
| Período de reset | Mensal |
| Alerta configurado | 80% do valor (tipo: Real/Actual) |
| Destinatário do alerta | E-mail próprio |

**Conceitos fixados:**

- Budget é apenas monitoramento/alerta — não bloqueia gasto automaticamente, para evitar interrupção acidental de serviços críticos em produção.
- Bloqueio automático de custo é possível apenas via configuração explícita adicional (Action Groups), nunca é o comportamento padrão.

## Domínio 1 — Status final

✅ Microsoft Entra ID (usuários, grupos de segurança) ✅ RBAC (Reader via grupo + Contributor individual, permissões cumulativas) ✅ Azure Policy (Locais permitidos, efeito Deny) ✅ Management Groups (hierarquia e herança) ✅ Cost Management (Budget com alerta)

**Domínio 1 do AZ-104 concluído na prática.** Próximo passo do roteiro: Domínio 2 — Implementar e gerenciar armazenamento.

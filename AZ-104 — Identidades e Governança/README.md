# Relatório de Exercício — AZ-104 Domínio 1
## Identidades e Governança: Microsoft Entra ID + RBAC

**Autor:** Dalgeni Burak
**Domínio do exame:** Manage Azure identities and governance (~20-25%)

---

## Objetivo

Praticar a criação de identidades no Microsoft Entra ID, agrupamento via grupo de segurança e atribuição de controle de acesso baseado em função (RBAC) em um Resource Group, entendendo o comportamento de herança e permissões cumulativas.

## Recursos criados

| Recurso | Tipo | Detalhe |
|---|---|---|
| `usuario.teste` | Usuário Entra ID | Membro, senha gerada automaticamente |
| `usuario.teste2` | Usuário Entra ID | Membro, senha gerada automaticamente |
| `GRP-TesteRBAC` | Grupo de Segurança | Tipo de associação: Atribuído (manual) |
| `RG-TesteRBAC` | Grupo de Recursos | Região: East US |

## Atribuições de RBAC realizadas

| Identidade | Função (Role) | Escopo | Via |
|---|---|---|---|
| `GRP-TesteRBAC` (grupo inteiro) | Leitor (Reader) | `RG-TesteRBAC` | Atribuição ao grupo |
| `usuario.teste` (individual) | Colaborador (Contributor) | `RG-TesteRBAC` | Atribuição direta ao usuário |

## Conceitos praticados

1. **Microsoft Entra ID vs Active Directory local** — Entra ID é o serviço de identidade para nuvem (OAuth2/OpenID Connect/SAML), enquanto o AD local usa protocolos de rede interna (Kerberos/LDAP). Em ambientes híbridos, os dois coexistem sincronizados via Entra Connect.

2. **OU vs Grupo de Segurança** — OU é estrutura organizacional/administrativa (aplica GPOs, organiza objetos); Grupo de Segurança é a estrutura usada para atribuir permissões de acesso. Esse mesmo princípio se replica no Azure: grupos de segurança do Entra ID são o que se atribui a recursos via RBAC.

3. **Grupo de Segurança (Entra ID) vs Grupo de Recursos (Resource Group)** — Apesar do nome parecido em português, são conceitos totalmente distintos: um agrupa **identidades**, o outro agrupa **recursos de infraestrutura**. RBAC é a ligação entre os dois.

4. **Hierarquia de herança no Azure** — Permissões atribuídas em um nível superior (ex: assinatura) herdam automaticamente para todos os níveis abaixo (Resource Groups e recursos), a menos que sobrescritas explicitamente em um nível mais específico. Boa prática: atribuir sempre no nível mais específico possível (**princípio do menor privilégio**).

5. **Permissões cumulativas no RBAC** — O Azure não tem negação implícita: o acesso efetivo de uma identidade é a **soma** de tudo que ela recebe, direta ou indiretamente (via grupo). Quando `usuario.teste` recebeu Contributor além do Reader herdado do grupo, seu acesso efetivo passou a ser Contributor (o nível mais amplo prevalece na prática, por soma, não por sobreposição).

6. **Grupos dinâmicos vs atribuídos** — Grupos de associação "Dinâmica" reavaliam automaticamente a membresia com base em regras (ex: atributo Departamento), reduzindo trabalho manual e risco de permissão esquecida — relevante em governança de identidade em escala.

## Resultado validado

- `usuario.teste2` (só Reader via grupo): consegue visualizar recursos do `RG-TesteRBAC`, mas não criar/editar/excluir.
- `usuario.teste` (Reader via grupo + Contributor direto): consegue criar/editar/excluir recursos — permissão efetiva de Contributor.

## Próximos passos (Domínio 1)

- Azure Policy — criar política de restrição de região e testar violação
- Management Groups e organização de múltiplas assinaturas
- Cost Management — configurar budget com alerta

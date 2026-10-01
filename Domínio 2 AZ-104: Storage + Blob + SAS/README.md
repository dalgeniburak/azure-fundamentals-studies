# Relatório de Exercício — AZ-104 Domínio 2 (parte 1)

## Armazenamento: Storage Account, Blob Storage e SAS Token

**Autor:** Dalgeni Burak **Domínio do exame:** Implement and manage storage (\~15-20%)

---

## Objetivo

Praticar a criação de uma Storage Account, uso do Blob Storage via container e geração de uma SAS Token para compartilhamento temporário e restrito de um arquivo.

## Recursos criados

| Recurso | Tipo | Detalhe |
| --- | --- | --- |
| `RG-TesteStorage` | Resource Group | Região East US |
| `sttestedalgeni` | Storage Account | Standard, LRS, East US |
| `container-teste` | Container (Blob Storage) | Nível de acesso: Private |
| `teste.txt` | Blob | Upload de teste, tier Hot (herdado) |

## SAS Token gerado

| Parâmetro | Valor |
| --- | --- |
| Permissão | Read (somente leitura) |
| Protocolo | HTTPS only |
| Validade | \~24 horas |

**Teste realizado:** acesso ao `teste.txt` via Blob SAS URL em aba anônima do navegador (sem login no tenant) — sucesso, confirmando acesso temporário e restrito sem expor o container inteiro.

## Conceitos praticados

1. **Redundância (LRS) não é backup/DR** — LRS protege contra falha de hardware local (3 cópias no mesmo data center), mas não contra perda do data center inteiro. Para isso, é necessário GRS/RA-GRS, com trade-off de custo.
2. **Tiers de acesso (Hot/Cool/Archive)** — equilíbrio entre custo de armazenamento e custo/velocidade de recuperação do dado.
3. **Acesso privado por padrão em containers Blob** — nenhum dado fica publicamente acessível sem configuração explícita, evitando exposição acidental.
4. **SAS Token como aplicação do menor privilégio no nível de dado** — assim como o RBAC restringe por identidade, o SAS Token restringe por arquivo/escopo, tempo e tipo de operação (leitura, escrita, exclusão), sem precisar expor a chave mestra da Storage Account.

## Próximos passos (Domínio 2)

- Azure Files (compartilhamento tipo SMB, análogo a drive-map de GPO local)
- Tiers de acesso na prática (mover blob entre Hot/Cool/Archive)
- Azure Backup (configurar e simular restauração)

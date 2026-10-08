# Relatório de Exercício — AZ-104 Domínio 4

## Redes Virtuais: VNet, Subnets, NSG, Peering e DNS Privado

**Autor:** Dalgeni Burak **Domínio do exame:** Configure and manage virtual networking (\~15-20%)

---

## Objetivo

Construir uma rede segmentada no Azure (web e banco de dados em subnets separadas), controlar o tráfego entre elas com um Network Security Group, conectar duas VNets por peering e resolver nomes internos com uma zona DNS privada. O VPN Gateway foi estudado apenas na teoria, por cobrar por hora desde a criação.

## 1. VNet e subnets

| Recurso | Detalhe |
| --- | --- |
| `vnet-teste-dalgeni` | Espaço de endereços 10.0.0.0/16, East US, `RG-TesteRede` |
| `default` | 10.0.0.0/24 |
| `subnet-db` | 10.0.1.0/24, private subnet (sem acesso de saída padrão à internet) |
| `subnet-web` | 10.0.2.0/24 |

**Conceitos praticados:**

- Subnets de uma mesma VNet não podem ter faixas sobrepostas.
- O Azure reserva 5 endereços por subnet (rede, gateway, dois de DNS e broadcast), então um /24 tem 251 IPs utilizáveis.
- A subnet é a unidade onde se aplicam NSG, tabela de rotas e NAT gateway. Separar web e banco permite dar tratamentos de rede diferentes a cada um e limita o estrago caso o servidor web seja comprometido (segmentação).

## 2. Network Security Group

| Prioridade | Nome | Regra |
| --- | --- | --- |
| 100 | `Allow-SQL-from-web` | Allow, TCP, porta 1433, origem 10.0.2.0/24 |
| 200 | `Deny-VNet-other` | Deny, qualquer porta/protocolo, origem VirtualNetwork |

O `nsg-db` foi associado à `subnet-db`. Confirmação: "Associated with: 1 subnets" na Overview do NSG.

**Conceitos praticados:**

- O NSG avalia as regras da menor para a maior prioridade e a primeira que combina decide. Uma regra que não combina é apenas ignorada.
- Toda NSG traz regras padrão que não podem ser apagadas. A `AllowVnetInBound` (65000) libera todo o tráfego dentro da VNet, por isso foi necessária a regra Deny de prioridade 200 para restringir o banco.
- A exceção específica (Allow) precisa ter prioridade maior que a regra ampla (Deny). Com as prioridades invertidas, o servidor web não conseguiria acessar o banco.
- Dois mecanismos distintos controlam a saída: a opção Private subnet (retira o acesso de saída padrão) e as regras de saída do NSG (a padrão `AllowInternetOutBound` libera a internet).

## 3. VNet Peering

| Recurso | Detalhe |
| --- | --- |
| `vnet-teste-2` | 10.1.0.0/16, East US, `RG-TesteRede` |
| Peering | Bidirecional entre `vnet-teste-dalgeni` e `vnet-teste-2`, status Connected / Fully Synchronized |

**Conceitos praticados:**

- Os espaços de endereço das VNets não podem se sobrepor, porque o mesmo endereço existiria nas duas redes e o roteamento ficaria ambíguo.
- O peering não é transitivo: se A conecta com B e B com C, A não alcança C sem um peering direto ou um recurso intermediário.
- O peering usa a rede da Microsoft, sem passar pela internet. Criar não tem custo, só há cobrança pela transferência de dados.

## 4. Private DNS Zone

| Recurso | Detalhe |
| --- | --- |
| Zona | `lab.interno` (Private DNS zone) |
| Links de VNet | `link-vnet1` (`vnet-teste-dalgeni`) e `link-vnet2` (`vnet-teste-2`), ambos Completed |
| Registro | `db` do tipo A apontando para 10.0.1.4 |

**Conceitos praticados:**

- Uma zona privada só é visível para as VNets que têm um link com ela. O link controla a resolução de nomes, o peering controla a rota entre as redes, e são independentes.
- Com auto-registration habilitado em um link, as VMs da VNet registram seus nomes na zona automaticamente. Cada VNet pode ter auto-registration em uma única zona.
- O TTL define por quanto tempo quem consulta pode guardar a resposta em cache. Um TTL muito alto faz clientes manterem um IP antigo se o endereço do banco mudar.

## 5. VPN Gateway (teoria)

- **Site-to-Site:** liga uma rede inteira (escritório, homelab) à VNet, de forma permanente.
- **Point-to-Site:** liga um único dispositivo (notebook em home office) à VNet.
- **VNet-to-VNet:** liga duas VNets por gateway. Entre VNets do Azure, o peering costuma ser mais simples e mais barato.
- Exige uma subnet dedicada chamada `GatewaySubnet`, cobra por hora e leva cerca de 30 a 45 minutos para ser criado.
- **ExpressRoute** é um circuito privado dedicado, que não passa pela internet pública.

## Observações práticas

- Conferir sempre o **tipo do recurso** que aparece logo abaixo do título da página. Por duas vezes o nome enganou: um NSG que na verdade era uma VNet, e uma DNS zone pública no lugar da Private DNS zone.
- A busca do Marketplace mistura serviços da Microsoft com VMs de terceiros cobradas por hora. Para recursos de rede, é mais seguro buscar pela barra de busca principal do portal.
- No registro DNS, conferir a unidade do campo TTL (segundos, minutos, horas ou dias).
- A primeira VNet foi parar no resource group padrão por não ter sido selecionado o grupo na criação, e foi recriada no `RG-TesteRede`.

## Domínio 4 — Status final

✅ VNet e subnets ✅ Network Security Group associado à subnet ✅ VNet Peering ✅ Private DNS Zone com links de VNet ✅ VPN Gateway (conceitual)

**Domínio 4 do AZ-104 concluído na prática.** Próximo passo do roteiro: Domínio 5 — Monitorar e fazer backup de recursos do Azure.

## Nota de custo

VNets, subnets, NSG e peering não têm custo de criação. A zona DNS privada custa centavos e deve ser apagada após o exercício.

# O que é um firewall?

<br/>

É um dispositivo de segurança (podendo ser um ser um hardware ou software) que monitora e controla o tráfego de rede, permitindo ou bloqueando ( o tráfego de pacotes de dados ou em portas) com base em regras predeterminadas, ele atua como uma barreira de segurança.

## Onde fica localizado em uma topologia?

<br/>

![Topologia firewall](https://github.com/peagaaa/Iptables-firewall/blob/main/assets/topologia-firewall.jpg)

<p align="center">
  <img src="https://github.com/peagaaa/Iptables-firewall/blob/main/assets/topologia-firewall.jpg" alt="...">
</p>

# Desafio proposto: 

<br/>

Escolher uma distribuição Linux e uma solução gratuita de Firewall para cumprir os requisitos:

- Bloqueio de site adulto
- Bloqueio de site específico
- Bloqueio de ataque DDOS 
- Fazer a configuração de rede local

<br/>

---

## Stack utilizada

<br/>

**Aplicativo para simular uma máquina virtual:** VirtualBox

**Distribuição linux:** Debian

**Firewall:** Iptables

<br/>

---

# Iptables:

<br/> 

Solução de Firewall que monitora o tráfego de rede decidindo quais conexões/ pacotes serão permitidos e/ou bloqueados, sua particularidade é que ele faz isso utilizando tabelas com conjunto de regras chamadas cadeias (chains).

### Tabelas:

#### Filter:
 
Filtrar o tráfego de pacotes com base em regras definidas.

#### Nat: 

usada para realizar a tradução de endereços de rede, geralmente usada para redirecionamento.

#### Mangle: 

usada para modificar os cabeçalhos dos pacotes. É usada principalmente para marcação de pacotes para posterior manipulação por outras regras ou ferramentas.

#### Raw: 

Essa tabela é usada para controle e rastreamento de conexões.

---

### Chains: 

#### Prerouting: 

Usada para Manipular pacotes de entrada antes de qualquer roteamento, esta cadeia é utilizada logo que um pacote entra no sistema, normalmente usada para alterar o destino de pacotes (Destination NAT ou DNAT) na tabela nat. 

#### Input: 

Controla o tráfego de pacotes que estão tentando entrar no sistema.

#### Forward: 

Controla o tráfego que está passando pelo sistema, mas não é destinado a ele, e o sistema está atuando como um roteador, repassando pacotes de uma rede para outra.

#### Output: 

Controla o tráfego de pacotes que estão tentando sair do sistema, sempre que o sistema envia um pacote de dados para fora, ele passa por esta cadeia.

#### Posrouting: 

Usada para manipular pacotes de saída após o roteamento ter sido decidido sem antes sair do sistema, também utilizada na tabela nat.

# Relação entre tabela x cadeia

<br/>

![Relação tabela x cadeia iptables](../Firewall\assets\iptables.jpg)

# Sintaxe básica:

<p style="text-align: center"> iptables -A (chain) -i (interface) -p (protocol) (tcp/udp) -s (source) --dport (port)  -j (target)​​<p\>
​
<br/>

# Comandos de exemplos:

- iptables -A INPUT -p tcp --dport 443 -j ACCEPT​
- iptables -A INPUT -i lo -j ACCEPT​
- iptables -P INPUT DROP​
- iptables -A INPUT -s 203.0.113.0 -j DROP​
- iptables -A INPUT -j LOG --log-prefix "Pacote descartado: " --log-level 4​
- iptables -A INPUT -s 203.0.113.0 -j DROP

---

<br/>

<p style="text-align: center">Solução dos desafios propostos<p\>

<br/>

# Bloqueio de site adulto:

A forma mais rápida de fazer isso é utilizando de servidores DNS de que já possuem esse bloqueio, os mais utilizados são o Family shield e o da cloudflare:

![ADCACD](../Firewall\assets\DNS-BLOCK.png)

Basta configurar o DNS direto na placa de rede do computador, dessa forma:






 mas utilizando o iptables ficaria mais ou menos assim:

![](../Firewall\assets\DNS-BLOCK2.png)

----
----

# Bloqueio de site específico com iptables:


explicação:

Peculiaridade: Se no navegador já tenha sido feito o acesso ao serviço antes de aplicar as regras o bloqueio não funciona entretanto isso é facilmente resolvido apagando o histórico do navegador, isso ocorre devido a memória cache do navegador.

# Bloqueio de site específico (forma mais try hard):



# Bloqueio de ataque DDOS

# Configuração de rede'

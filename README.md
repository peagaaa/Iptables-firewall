# O que é um firewall?

É um dispositivo de segurança (podendo ser um ser um hardware ou software) que monitora e controla o tráfego de rede, permitindo ou bloqueando ( o tráfego de pacotes de dados ou em portas) com base em regras predeterminadas, ele atua como uma barreira de segurança.

## Onde fica localizado em uma topologia?

<p align="center">
  <img src="https://github.com/peagaaa/Iptables-firewall/blob/main/assets/topologia-firewall.jpg" alt="Imagem topologia firewall">
</p>

# Desafio proposto: 

Escolher uma distribuição Linux e uma solução gratuita de Firewall para cumprir os requisitos:

- Bloqueio de site adulto
- Bloqueio de site específico
- Bloqueio de ataque DDOS 
- Fazer a configuração de rede local

#  Stack utilizada

**Aplicativo para simular uma máquina virtual:** VirtualBox

**Distribuição linux:** Debian

**Firewall:** Iptables

# Iptables:

Solução de Firewall que monitora o tráfego de rede decidindo quais conexões/ pacotes serão permitidos e/ou bloqueados, sua particularidade é que ele faz isso utilizando tabelas com conjunto de regras chamadas cadeias (chains).

## Tabelas:

#### Filter:
 
Filtrar o tráfego de pacotes com base em regras definidas.

#### Nat: 

usada para realizar a tradução de endereços de rede, geralmente usada para redirecionamento.

#### Mangle: 

usada para modificar os cabeçalhos dos pacotes. É usada principalmente para marcação de pacotes para posterior manipulação por outras regras ou ferramentas.

#### Raw: 

Essa tabela é usada para controle e rastreamento de conexões.

## Chains: 

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

<p align="center">
  <img src="https://github.com/peagaaa/Iptables-firewall/blob/main/assets/iptables.jpg" alt="Imagem relação tabela x cadeia">
</p>

# Sintaxe básica:

` iptables -A (chain) -i (interface) -p (protocol) (tcp/udp) -s (source) --dport (port)  -j (target) `

# Comandos de exemplos:

```
iptables -A INPUT -p tcp --dport 443 -j ACCEPT​
iptables -A INPUT -i lo -j ACCEPT​
iptables -P INPUT DROP​
iptables -A INPUT -s 203.0.113.0 -j DROP​
iptables -A INPUT -j LOG --log-prefix "Pacote descartado: " --log-level 4​
iptables -A INPUT -s 203.0.113.0 -j DROP
```

<p style="text-align: center">Solução dos desafios propostos<p\>

# Bloqueio de site adulto:

A forma mais rápida de fazer isso é utilizando de servidores DNS de que já possuem esse bloqueio, os mais utilizados são o Family shield e o da cloudflare:

<p align="center">
  <img src="https://github.com/peagaaa/Iptables-firewall/blob/main/assets/DNS-BLOCK.png" alt="Servidores de DNS para bloqueio de site adulto">
</p>

Basta configurar o DNS direto na placa de rede do computador, dessa forma:

<p align="center">
  <img src="https://github.com/peagaaa/Iptables-firewall/blob/main/assets/mudarDNSplacaderede.png" alt="Alteração de DNS placa de rede Win 10">
</p>

 mas utilizando o iptables o fica dessa forma:

<p align="center">
  <img src="https://github.com/peagaaa/Iptables-firewall/blob/main/assets/comandoBlock.png" alt="Comando iptables bloquear site adulto">
</p>

# Bloqueio de site específico com iptables:

Para cumprir com esse requisito, após muitas pesquisas consegui achar um artigo que fala sobre bloqueio a determinados serviços com o Iptables:

[Link do artigo!](https://dejano.comunidades.net/bloqueando-facebookhttps-via-iptables)

De exemplo iremos bloquear o acesso ao TikTok

Segue os comandos: 

```
iptables -I FORWARD -m string --algo bm --string "tiktok.com" -j DROP
iptables -I OUTPUT -m string --algo bm --string "tiktok.com" -j 
```

Explicação:

Peculiaridade: Se no navegador já tenha acessado serviço antes de aplicar as regras, o bloqueio não irá funcionar, entretanto isso é facilmente resolvido apagando o histórico do navegador, isso ocorre devido a memória cache do navegador.

# Bloqueio de site específico (forma mais try hard):

# Bloqueio de ataque DDOS

# Configuração de rede

Vamos lá, como o Iptables atuaria como um firewall na rede?

A ideia da utilização de um firewall seria de que todo o tráfego da rede passe por ele antes de ir para a rede interna ou seja encaminhado para a internet. No meu exemplo optei por utilizar uma VM, então eu teria uma máquina como cliente e um servidor que estaria hospedando o meu firewall, a topologia nesse caso ficaria assim: 

<p align="center">
  <img src="https://github.com/peagaaa/Iptables-firewall/blob/main/assets/top.png" alt="Topologia firewall">
</p>

o meu servidor deverá ter duas placas de rede uma para se comunicar com a rede interna (Lan | com ip fixo) e outra para se comunicar com a rede externa (Wan | internet | DHCP ativo).

No VirtualBox após habilitar a segunda placa de rede, é necessário configura-las a LAN como Host only e a WAN como bridge.

Também há a necessidade de realizar algumas configurações no servidor linux, são elas:

----

Após essas configurações no servidor vamos partir para a máquina do cliente, na configuração de rede iremos configurar o gateway com o IP do servidor, a marcara de sub-rede como pardrão `255.255.255.0` e o IP como um endereço dentro da faixa especificada. 





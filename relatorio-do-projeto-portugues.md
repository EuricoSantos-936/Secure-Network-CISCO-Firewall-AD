# Relatório do Projeto - Português

**1. Objetivo do Projeto**

O objetivo deste projeto é criar uma rede de computadores segmentada, segura e funcional usando o Cisco Packet Tracer 8.2.2. A rede inclui um IDS (simulado com ACLs), ligação à internet, um servidor de email, um servidor de Active Directory, uma rede wireless com dispositivos móveis, e uma firewall para segurança adicional.

**2. Componentes da Rede**

* **Router Cisco 4331 ISR**
* **Firewall Cisco ASA 5505**
* **Switch Layer 3 Cisco Catalyst 3560**
* **Switch Layer 2 (para cada segmento de rede)**
* **Servidores (IDS simulado, Email/AD)**
* **PCs e Dispositivos Móveis (Smartphones e Laptops)**
* **Access Point para rede wireless**

**3. Topologia da Rede**

<figure><img src=".gitbook/assets/Captura de ecrã de 2024-07-04 21-46-04.png" alt=""><figcaption></figcaption></figure>

**4. Configurações dos Dispositivos**

**Router Cisco 4331 ISR**

* **Interface GigabitEthernet0/0:**
  * IP Address: 192.168.1.1 (conectado ao ISP)
* **Interface GigabitEthernet0/1:**
  * IP Address: 192.168.4.1 (conectado à firewall)

**Firewall Cisco ASA 5505**

* **Interface Ethernet0/0:**
  * IP Address: 192.168.4.2 (conectado ao roteador)
* **Interface Ethernet0/1:**
  * Conectada ao Switch Layer 3

**Switch Layer 3 Cisco Catalyst 3560**

* **Interface GigabitEthernet0/1:**
  * Conectado à firewall
* **Configuração de VLANs:**
  * VLAN 10:  Vendas (192.168.2.0/24)
  * VLAN 20: Finanças (192.168.3.0/24)
  * VLAN 30: DMZ (192.168.4.0/24)
  * VLAN 40: Rede Wireless (192.168.5.0/24)

**Switches de Acesso**

* **Switch1 (Vendas):**
  * Conectado ao Switch Layer 3
  * VLAN 10
* **Switch2 (Finanças):**
  * Conectado ao Switch Layer 3
  * VLAN 20
* **Switch3 (DMZ):**
  * Conectado ao Switch Layer 3
  * VLAN 30
* **Switch4 (Wireless):**
  * Conectado ao Switch Layer 3 e ao Access Point
  * VLAN 40

**Servidores**

* **Servidor de Email/AD:**
  * IP Address: 192.168.4.200
  * Serviços: Active Directory (AAA), Email
* **Servidor IDS Simulado:**
  * Simulação de IDS com ACLs no roteador/firewall

**5. Configuração dos Dispositivos Finais**

**PCs e Dispositivos Móveis**

* **Configuração de IPs Estáticos:**
  * PCs na Rede de Vendas (192.168.2.x):
    * Default Gateway: 192.168.2.254
    * DNS Server: 192.168.4.200
  * PCs na Rede de Finanças (192.168.3.x):
    * Default Gateway: 192.168.3.254
    * DNS Server: 192.168.4.200
  * Dispositivos na DMZ (192.168.4.x):
    * Default Gateway: 192.168.4.254
    * DNS Server: 192.168.4.200
  * Dispositivos na Rede Wireless (192.168.5.x):
    * Default Gateway: 192.168.5.254
    * DNS Server: 192.168.4.200

**6. Configuração de ACLs para Simulação de IDS**

**No Router**

```
Router> enable
Router# configure terminal
! Criar ACL para permitir tráfego HTTP/HTTPS
Router(config)# access-list 100 permit tcp any any eq 80
Router(config)# access-list 100 permit tcp any any eq 443
! Criar ACL para negar todo o tráfego ICMP (ping)
Router(config)# access-list 100 deny icmp any any
! Permitir todo o outro tráfego
Router(config)# access-list 100 permit ip any any
! Aplicar ACL à interface interna
Router(config)# interface gigabitEthernet0/1
Router(config-if)# ip access-group 100 in
Router(config-if)# exit
Router(config)# exit
```

Na Firewall

```
Router> enable
Router# configure terminal
Router(config)# access-list 100 deny icmp any any
Router(config)# end
!Esta ACL nega todo o tráfego ICMP, comumente usado para operações de ping, para simular o comportamento de IDS 
bloqueando atividades de reconhecimento potenciais.
```

**7. Teste e Verificação**

* **Conectividade Interna:**
  * Testar ping entre PCs nas diferentes redes.
* **Acesso à Internet:**
  * Verificar conectividade com o servidor ISP.
* **Funcionalidade de Email/AD:**
  * Autenticar usuários no servidor AD e enviar emails entre contas configuradas.
* **Monitoramento de ACLs:**
  * Verificar os logs para tráfego permitido/negado conforme as ACLs.

#### Conclusão

Este projeto configura uma rede segura e segmentada com conectividade interna e externa adequada. A utilização de ACLs para simulação de IDS fornece uma camada adicional de monitoramento e segurança, enquanto os servidores de Email/AD garantem a funcionalidade essencial de comunicação e autenticação de usuários.

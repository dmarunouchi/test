
### **Tarefa 2 - Configurando o IP para a interface Vlan1:**

Vamos configurar o IP 10.1.10.250/24 que será utilizado como gateway desta rede.

Entre na configuração da interface Vlan1:
```
[HPE]interface Vlan-interface1
```
Insira o IP e a máscara de rede no prefixo CIDR (Classless Inter-Domain Routing):
```
[HPE-Vlan-interface1]ip address 10.1.10.250 24
```
Valide as configurações aplicadas (de acordo com a Lei de Murphy: **_se pode dar merda, vai dar merda!_** kkk 😁)
```
[HPE-Vlan-interface1]display interface brief
Brief information on interface(s) under route mode:
Link: ADM - administratively down; Stby - standby
Protocol: (s) - spoofing
Interface Link Protocol Main IP Description
GE0/0 UP UP 192.168.0.21
GE0/5 DOWN DOWN --
InLoop0 UP UP(s) --
NULL0 UP UP(s) --
REG0 UP -- --
**Vlan1 UP UP 10.1.10.250**
Brief information on interface(s) under bridge mode:
Link: ADM - administratively down; Stby - standby
Speed or Duplex: (a)/A - auto; H - half; F - full
Type: A - access; T - trunk; H - hybrid
Interface Link Speed Duplex Type PVID Description
GE0/1 DOWN auto A A 1
GE0/2 UP 100M(a) F(a) A 1
GE0/3 DOWN auto A A 1
GE0/4 DOWN auto A A 1
```
Após validar que a configuração de IP para Vlan1 foi bem sucedida, podemos continuar com o próximo passo.

### **Tarefa 3 - Criar um DHCP Pool para ser utilizado na Vlan1:**

Para que dispositivos na sua rede recebam endereços IP automaticamente, precisamos configurar o roteador para atuar como um DHCP server.

Em nosso caso queremos que dispositivos da nossa rede recebam a seguinte configuração:

- IP da rede 10.1.10.0/24 começando do IP 10.1.10.50 até 10.1.10.200 (este será nosso "range")
- Gateway: 10.1.10.250 (o mesmo endereço que especificamos para a VLan1 no paço anterior)
- Servidores DNS: 1.0.0.1 e 1.1.1.1 (Neste caso estamos usando servidores DNS públicos, mas poderíamos utilizar Servidores locais)

Aqui utilizaremos "pool-vlan1" como nome para o DHCP Pool
```
[HPE-Vlan-interface1]dhcp server ip-pool pool-vlan1
```
Defina o range de IP a ser utilizado (IP inicial e IP final)
```
[HPE-dhcp-pool-pool-vlan1]address range 10.1.10.50 10.1.10.200
```

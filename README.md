# Home-lab Project.

<img width="2800" height="2020" alt="home_lab_network_topology_v3" src="https://github.com/user-attachments/assets/a5dfa0c9-e741-4aa0-8dc7-5314792332d1" />


____

### -Network Architecture. (Arquitectura de Red.)

Three isolated zones behind a pfSense firewall — WAN (attacker), LAN (domain-joined
assets), DMZ (exposed web app) — plus a dedicated MGMT zone for SIEM tooling. Every
path shown here has been tested end-to-end, not just configured. (Tres zonas aisladas
detrás de un firewall pfSense — WAN (atacante), LAN (activos unidos al dominio), DMZ
(aplicación web expuesta) — más una zona MGMT dedicada para herramientas SIEM. Cada
ruta mostrada aquí fue probada de extremo a extremo, no solo configurada.)

____

### -Learning by Building. (Aprender Haciendo.)

This are some of the concepts and operative systems and tools' behaviors this project has taught me so far

(Estos son algunos de los conceptos y comportamientos de los sistemas operativos y herramientas que eh aprendido en este proyecto hasta el momento):

____

- The default restriction on Active Directory Domain Control -ADDC-, keeping standard accounts from log in, in order to mantain secure NTDS.dit database. Standard users can't log on via an interactive session on Domain Control, but must do it over the network via Kerberos/LDAP -Lightweight Directory Access Protocol-

  (Las restricciones predeterminadas del servidor ADDC mantienen fuera de acceso cuentas estándar para mantener segura la base de datos NTDS.dit. Los usuarios estándar no pueden accersar al servidor DC mediante sesión interactiva, deben hacerlo por mediante internet via Kerberos/LDAP.)
  
  <img width="60%" alt="Screenshot_2026-07-18_01-05-54" src="https://github.com/user-attachments/assets/7594d2dc-f614-42cc-8113-6138569561f8" />

____

**The DMZ <--> MGMT connection conflict. (Conflicto de comunicación DMZ <--> MGMT.)**

- pfSense up and running

  <img width="60%" alt="Screenshot_2026-07-24_10-43-54" src="https://github.com/user-attachments/assets/b390abef-18db-4107-bf47-ec4e3e5c9000" />

- Each network connected to pfSense firewall, WAN 10.10.0.1/24 Kali linux 10.10.0.10 external threat, MGMT 172.16.1.1/24 Ubuntu-Wazuh 172.16.1.10, it's tcp port 1514 opened to collect event's data, logs and warnings from running agents from DMZ 172.16.0.1/24 Debian-DVWA 172.16.0.10 and from LAN 192.168.200.1/24 Windows 11 Enterprise 192.168.200.40 and ADDC MS Server 2025 192.168.200.10, tcp port 1515 for agent's authentication and port 443 open to use Windows 11 as SIEM Management Console, ports opened through pfSense firewall rules.

  (Cada red conectada a pfSense firewall, WAN 10.10.0.1/24 Kali linux 10.10.0.10 amenza externa, MGMT 172.16.1.1/24 Ubuntu-Wazuh 172.16.1.10 Con el puerto tcp 1514 para colectar datos de eventos registros y advertencias enviadas por agentes activos, provenientes de DMZ 172.16.0.1/24 Debian-DVWA 172.16.0.10, y de LAN 192.168.200.1/24 Windows 11 Enterprise 192.168.200.40 y ADDC MS Server 2025 192.168.200.10, el puerto tcp 1515 para la autenticación de agentes, y el puerto 443 abierto para usar Windows 11 como consola de administración SIEM, puertos abiertos mediante reglas en pfSense firewall.)

  LAN:

  <img width="60%" alt="Screenshot_2026-07-24_14-03-22" src="https://github.com/user-attachments/assets/b5f5b7b4-b900-486a-a95e-fb82ec459547" />

  DMZ:

  <img width="60%" alt="Screenshot_2026-07-23_21-31-19" src="https://github.com/user-attachments/assets/adadd00e-d4c0-4eed-b9ae-9ab8fe13f3fc" />

- **Testing connection between MGMT and LAN and MGMT and DMZ**

  **(Probando la conexión entre MGMT y LAN y MGMT y DMZ)**

  Windows to Ubuntu-Wazuh ports 443, 1514 and 1515: Working properly.

  (De Windows a Ubuntu-Wazuh los puertos 443, 1514 y 1515: Funcionando apropiadamente)
 
  <img width="60%" alt="Screenshot_2026-07-23_22-47-34" src="https://github.com/user-attachments/assets/40858b96-750f-40d9-816d-b75d114af801" />

- Debian-DVWA to Ubuntu-Wazuh ports 1514 and 1515: Working properly
  
  (De Debian-DVWA a Ubuntu-Wazuh puertos 1514 y 1515: Funcionando apropiadamente)
  
  <img width="60%" alt="Screenshot_2026-07-23_18-01-19" src="https://github.com/user-attachments/assets/2b3a98b9-4b6f-49f4-b088-378bf32017ff" />

- After rebooting: ports 1514 and 1515 unexpectedly started refusing connections — the actual bug. Port 443 refused too, but that one's expected: DMZ was never supposed to reach the dashboard in the first place.

  (Después de reiniciar: los puertos 1514 y 1515 empezaron a rechazar conexiones inesperadamente — el error real. El puerto 443 también fue rechazado, pero eso es lo esperado: DMZ nunca debía poder alcanzar el dashboard.)

  <img width="60%" alt="Screenshot_2026-07-23_19-16-07" src="https://github.com/user-attachments/assets/ec250bd5-df00-486f-85e4-a20af8289336" />

- After looking out through pfSense firewall rules searching for configuration mistakes everything was clean, next step was checking that both Debian-DVWA and Ubuntu-Wazuh's firewall were disable to rule out they were responsible for the connection bug.

  (Despúes de revisar entre las reglas establecidas en pfSense firewall no se encontraron errores de configuración, el siguiente paso fue revisar que los firewall tanto de Debian-DVWA como Ubuntu-Wazuh estuviesen inactivos para descartar que fuesen los causantes del error de conexión.)

  <img width="60%" alt="Screenshot_2026-07-23_19-19-19" src="https://github.com/user-attachments/assets/155d0194-28f7-498e-a349-3d92c76c4614" />

 - Ubuntu-Wazuh firewall disable, then running command ss -tulpn (socket statistics) to check the network and ports' status on Wazuh, all 3 ports listening.

   (Firewall de Ubuntu-Wazuh deshabilitado, luego se corre el comando ss -tulpn (estadísticas del socket) para verificar el estado de la red y los puertos de Wazuh, los 3 puertos en funcionamiento.)

   <img width="60%" alt="Screenshot_2026-07-22_18-49-34" src="https://github.com/user-attachments/assets/f0a677cc-2572-47b6-a7e6-532c3d80a303" />

- The next test was reconfigurate the IP address on pfSense


  (La siguiente prueba fue mediante la reconfiguración de la dirección IP en pfSense)

  <img width="60%" alt="Screenshot_2026-07-24_22-46-00" src="https://github.com/user-attachments/assets/5e5f9cb9-5e35-4d53-83b2-37f5bf000693" />


- After a reboot once again at connection testing between Devian-DVWA and Ubuntu-Wazuh this was refused. The next tests were using pfSense own diagnostic tools on the webgui.

  (Despúes de reiniciar nuevamente al probar la conexion desde Debian-DVWA a Ubuntu-Wazuh esta era rechazada. Las siguientes pruebas se hicieron utilizando las propias herramientas de diagnostico propias de pfSense desde su interfaz gráfica.)

  MGMT PING --> DMZ

  <img width="60%" alt="Screenshot_2026-07-23_17-45-19" src="https://github.com/user-attachments/assets/f136b3e3-00b4-40a8-817c-429fd3a38005" />

  DMZ PING--> MGMT
  
  <img width="60%" alt="Screenshot_2026-07-23_17-59-18" src="https://github.com/user-attachments/assets/69958619-6b83-416d-be54-b439a7ba2e9c" />

  On both ping test was succesful, both 0% packet lost.

  (En ambas pruebas el ping fue exitoso, 0% de perdida de paquetes.)

  The next diagnostic tool was ARP table, looking for dicrepancys or anomalies.

  (La siguiente herramienta de diagnóstico era ARP table, para buscar dicrepancias o anomalias)

  <img width="60%" alt="Screenshot_2026-07-24_10-47-04" src="https://github.com/user-attachments/assets/64becdfb-6c23-4b9c-bd10-af29001105eb" />

  **Here's the lead anomaly. First, this would be normal behavior (and will use LAN and Windos as examples), LAN 192.168.200.1 was assigned on pfSense as the default gateway for all devices connected to pfSense on LAN, every device on LAN with an IP address in 192.168.200.1/24 range and 192.168.200.1 as gateway will call to pfSense like as its router's gateway, pfSense assigned this gateway 192.168.200.1 with the MAC address: 52:54:00:71:bb:23 and Windows 192.168.200.40 is correctly displaying it's NIC's MAC addres 52:54:00:e1:98:0f. both of them had their respective MAC addresses displayed on the ARP table.
  Now the anomaly, Debian-DVWA and Ubuntu-Wazuh that were pinging each other (theorically) through prSense after windows connection was established with pfSense, don't have their respective NIC's MACs displayed on the ARP table, We can see DMZ's gateway MAC address 52:54:00:63:63:46, and MGMT's gateway MAC 52:54:00:90:dd:1f. Then why can't we see Debian-DVWA and Ubuntu-Wazuh NIC's MACs?**

  **(Acá está la anomalía principal. Primero, este sería el comportamiento normal (y voy a utilizar LAN y a Windows como ejemplos), LAN 192.168.200.1 fue asignada en pfSense como el gateway predeterminado para los equipos conectados a pfSense a travez de LAN, cada equipo con una dirección IP en el rango 192.168.200.1/24 y que usen de gateway predeterminado 192.168.200.1 llamaran a mediante este acceso a pfSense como llamando al gateway de un router, pfSense le asignó a este gateway 192.168.200.1 la dirección MAC 52:54:00:71:bb:23, y Windows 192.168.200.40 muestra la direccion MAC de su NIC 52:54:00:e1:98:0f, ambos muestran sus respectivas MACs en la tabla ARP, podemos ver LA dirección MAC del gateway de DMZ 52:54:00:63:63:46 y la de MGMT 52:54:00:90:dd:1f. Entonces la pregunta es por qué no aparecen las direcciones MAC de Debian-DVWA y Ubuntu-Wazuh en la tabla ARP?)**

- One last corroboration, Using command apr -an on pfSense's shell to be sure there's no discrepancies with what was shown on WebGui.
  
  (Una última corroboración, utilizando el comando arp -an en la linea de comandos en pfSense para asegurarse que no hay discrepancias con lo mostrado en la interfaz de la web)
  
  <img width="60%" alt="Screenshot_2026-07-24_10-54-32" src="https://github.com/user-attachments/assets/f3aedf84-0003-49f5-a614-d27ffffbd78f" />

  No discrepancies.

- Explanation on Normal behavior, DMZ is an isolated network, with only 2 devices in it, Debian-DVWA and the router (pfSense) the only possible connection inside this netwerk is between Debian-DVWA and the router, when Debian-DVWA sends a ARP request the only device listening to it and available to answer it's the router. Using the command ip neighbor will ask to all device listening for the MAC address of the device with this IPv4 address 172.16.0.1, only the device using that IP address will answer back his MAC address... This is it. Problem found.

  (Explicación del comportamiento normal, DMZ es una red aislada, con solo 2 dispositivos, Debian-DVWA y un router (pfSense) la única conexión posible dentro de esta red es entre Debian-DVWA y el router, cuando Debian-DVWA envia un pedido ARP el unico equipo escuchando y con posibilidad de respuesta es el router. Usando el comando ip neighbor se envia un pedido de respuesta de la MAC hacia todos los equipos escuchando para que el equipo que tiene asignada la dirección IPv4 responda de vuelta su dirección MAC... Y aquí está, problema encontrado.

  <img width="60%" alt="Screenshot_2026-07-24_10-56-49" src="https://github.com/user-attachments/assets/8cbfbb73-0609-40cf-b92c-a6174f18752d" />

- What's happening here? There's a conflict, the same IP address is being used as default gateway for the bridge between the virtual network and the host, in this case Fedora as seen on *virbr3*, and the default gateway on pfSense.

  ()

  <img width="60%" alt="Screenshot_2026-07-25_18-11-44" src="https://github.com/user-attachments/assets/6e286906-2f90-4f79-8cd6-50ff9f31d8c1" />

= The solution, there are 2 solutions, 1 change the default IP address on virt manager from 172.16.0.1/24 to an IP address different than 172.16.0.10/24 in the same range.
  Solution 2, remove entire the IP address from bridging with the host, it's the mor sensible solution for the project since a total isolation provides less future problems and data will only flow were it should.

  
  <img width="60%" alt="Screenshot_2026-07-24_11-05-51" src="https://github.com/user-attachments/assets/687febf6-1d38-48ec-9086-ab19ed1bf0f7" />

### -A Note on AI Assistance. (Nota sobre la asistencia de IA.)

Claude (Anthropic) and Gemini (Google) were used throughout this build as technical
thinking partners — explaining networking and security concepts as they came up,
suggesting hypotheses to test when something broke, and helping generate the
diagrams and structure this writeup. Every command, every test, and every
configuration change was run and verified by hand. The root-cause diagnosis of the
ARP conflict above — cross-referencing pfSense's and Debian's ARP tables against the
libvirt network configs to find the duplicate Layer 2 responder — was independent
work.

(Claude (Anthropic) y Gemini (Google) se usaron a lo largo de este proyecto como
compañeros de pensamiento técnico — explicando conceptos de redes y seguridad a
medida que surgían, sugiriendo hipótesis para probar cuando algo fallaba, y
ayudando a generar los diagramas y estructurar este documento. Cada comando, cada
prueba y cada cambio de configuración fue ejecutado y verificado a mano. El
diagnóstico de la causa raíz del conflicto ARP arriba — comparando las tablas ARP
de pfSense y Debian con las configuraciones de red de libvirt para encontrar el
respondedor duplicado de Capa 2 — fue trabajo independiente.)



  

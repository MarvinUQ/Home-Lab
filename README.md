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

- Each network connected to pfSense firewall, WAN 10.10.0.1/24 Kali linux 10.10.0.10 external threat, MGMT 172.16.1.1/24 Ubuntu-Wazuh 172.16.1.10, it's tcp port 1514 opened to collect event's data, logs and warnings from running agents from DMZ 172.16.0.1/24 Debian DVWA 172.16.0.10 and from LAN 192.168.200.1/24 Windows 11 Enterprise 192.168.200.40 and ADDC MS Server 2025 192.168.200.10, tcp port 1515 for agent's authentication and port 443 open to use Windows 11 as SIEM Management Console, ports opened through pfSense firewall rules.

  (Cada red conectada a pfSense firewall, WAN 10.10.0.1/24 Kali linux 10.10.0.10 amenza externa, MGMT 172.16.1.1/24 Ubuntu-Wazuh 172.16.1.10 Con el puerto tcp 1514 para colectar datos de eventos registros y advertencias enviadas por agentes activos, provenientes de DMZ 172.16.0.1/24 Debian DVWA 172.16.0.10, y de LAN 192.168.200.1/24 Windows 11 Enterprise 192.168.200.40 y ADDC MS Server 2025 192.168.200.10, el puerto tcp 1515 para la autenticación de agentes, y el puerto 443 abierto para usar Windows 11 como consola de administración SIEM, puertos abiertos mediante reglas en pfSense firewall.)

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

- After looking out through pfSense firewall rules searching for configuration mistakes everything was clean, next step was checking that both Debian and Ubuntu's firewall were disable to rule out they were responsible for the connection bug.

  (Despúes de revisar entre las reglas establecidas en pfSense firewall no se encontraron errores de configuración, el siguiente paso fue revisar que los firewall tanto de Debian como Ubuntu estuviesen inactivos para descartar que fuesen los causantes del error de conexión.)
  
<img width="60%" alt="Screenshot_2026-07-23_19-07-27" src="https://github.com/user-attachments/assets/332f3127-0594-406c-b725-366a592590d0" />

<img width="60" alt="Screenshot_2026-07-22_18-49-34" src="https://github.com/user-attachments/assets/f0a677cc-2572-47b6-a7e6-532c3d80a303" />


- 







  

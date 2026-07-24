# Homelab Project.



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

This are some of the concepts and operative systems and tools' behaviors this project has taught me so far (Estos son algunos de los conceptos y comportamientos de los sistemas operativos y herramientas que eh aprendido en este proyecto hasta el momento):

____

- The default restriction on Active Directory Domain Control -ADDC-, keeping standard accounts from log in, in order to mantain secure NTDS.dit database. Standard users can't log on via an interactive session on Domain Control, but must do it over the network via Kerberos/LDAP -Lightweight Directory Access Protocol- (Las restricciones predeterminadas del servidor ADDC mantienen fuera de acceso cuentas estándar para mantener segura la base de datos NTDS.dit. Los usuarios estándar no pueden accersar al servidor DC mediante sesión interactiva, deben hacerlo por mediante internet via Kerberos/LDAP.)
  
<img width="80%" alt="Screenshot_2026-07-18_01-05-54" src="https://github.com/user-attachments/assets/7594d2dc-f614-42cc-8113-6138569561f8" />

____

**The DMZ <--> MGMT connection conflict. (Conflicto de comunicación DMZ <--> MGMT.)**

- With the MGMT network up and connected to pfSense and with 1514-1515 port opened to collect data from DMZ 172.16.0.1/24 Debian DVWA 172.16.0.10 and from LAN 102.168.200.1/24 Windows 11 Enterprise 102.168.200.40 and ADDC MS Server 2025 102.168.200.10, the connetion from Windos worked as it should

<img width="80%" alt="Screenshot_2026-07-23_22-47-34" src="https://github.com/user-attachments/assets/40858b96-750f-40d9-816d-b75d114af801" />
  

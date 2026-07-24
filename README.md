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

- The default restriction on Active Directory Domain Control -ADDC-, keeping standard accounts from log in, in order to mantain secure NTDS.dit database. Standard users can't log on via an interactive session on Domain Control, but must do it over the network via Kerberos/LDAP -Lightweight Directory Access Protocol- (Las restricciones predeterminadas del servidor ADDC mantienen fuera de acceso cuentas estándar para mantener segura la base de datos NTDS.dit. Los usuarios estándar no pueden accersar al servidor DC mediante sesión interactiva, deben hacerlo por mediante internet via Kerberos/LDAP.)
  
<img width="75%" alt="Screenshot_2026-07-18_01-05-54" src="https://github.com/user-attachments/assets/7594d2dc-f614-42cc-8113-6138569561f8" />

- The DMZ <--> MGMT connection conflict. (Conflicto de comunicación DMZ <--> MGMT.)

# -Learned

### - On ADDC Server behavior. (Comportamiento de servidor ADDC.)

- The default restriction on Active Directory Domain Controller -ADDC-, keeping standard accounts from logging in, in order to maintain a secure NTDS.dit database. Standard users can't log in via an interactive session on Domain Controller, but must do it over the network via Kerberos/LDAP -Lightweight Directory Access Protocol-

  (Las restricciones predeterminadas del servidor ADDC mantienen fuera de acceso cuentas estándar para mantener segura la base de datos NTDS.dit. Los usuarios estándar no pueden acceder al servidor DC mediante sesión interactiva, deben hacerlo a través de la red vía Kerberos/LDAP.)
  
<img src="Images/LbB-ADDC-01.png" alt="ADDC01" width="70%">

____

## Adding agentless monitoring on pfSense to Wazuh using Syslog. (Agregar monitoreo sin agentes a pfSense hacia Wazuh usando Syslog.)

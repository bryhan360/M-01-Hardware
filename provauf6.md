omandes per instal.lar el servidor web
    
      

2. Comandes per activar el servei web
        
      - systemctl status sshd ==> mira el estado en que se encuentra el servicio web
     
      - systemctl start sshd ==> Activa el servicio web

3. Comandes per activar el tallafocs firewalld

      - systemctl start firewalld ==> Activa el Firewall

4. Comandes per donar accés remot al servidor web.

      - firewall-cmd --zones=FedoraWorkstation --add-port=80/tcp

#Prova pràctica UF6


1.   Comandes per instal.lar el servidor web

     - dnf install -y nginx
     
     - systemctl active nginx.service

2.   Comandes per activar el servei web

     - systemctl start nginx.service

3.   Comandes per activar el tallafocs firewalld

     - systemctl start firewalld.service

4.   Comandes per donar accés remot al servidor web.
    
     - systemctl restart firewalld.service

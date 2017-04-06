# Gestion de Volumens Logicos

__1. Que es un Gestor Volumen Logico?__

Es una capa de abstraccion entre un  dispositivo de almacenamiento ( Un disco Duro ) y un sistema de ficheros. 

* __Que quieren decir las siglas, definición, analogias y ejemplos de comandos (explicando que hacen) vistos en clase:__

    * __PV :__  El Physical Volume o Volumen Fisico, es un
dispositivo de bloques. Puede ser un disco duro, una particion, una tarjeta SD, un floppy, un dispositivo RAID, un dispositivo loop (Convierte un fichero a un dispositivo de bloque), un dispositivo cifrado, hasta un volumen logico puede usarse de PV.   
___-pvcreate /dev/vda 500M ==> Este comando sirve para crear un Volumen Fisico (PV) partidiendo primero del destino y seguido el tamaño___  

        ___-pvs ==> Este comando nos muestra informacion acerca del Volumen Fisico en nuestra maquina___

        ```
        [root@Bryhan ~]# pvs
          PV         VG     Fmt  Attr PSize   PFree  
        /dev/sda2  fedora lvm2 a--    9,51g      0 
        /dev/vda          lvm2 ---  204,80m 204,80m
        ```

    * __VG :__ El Volume Group o Grupo de Volumenes usa el espacio almacenado de un PV, este deb pertenecer a un Grupo de volúmenes Se puede decir que un VG es una especie de disco duro virtual.Un VG es un “disco” compuesto de UNO o más PVs y que crece simplemente añadiendo más PVs. A diferencia de un disco real, un VG puede crecer con el tiempo, sólo hay que “darle” un PV más.
    
      ___-vgcreate multimedia /dev/vda ==> Con esto creamos un grupo de volumenes (VG)___
      
      ___-vgs ==> Muentra informacion sobre el volumen de grupos___

        ```
        [root@Bryhan ~]# vgs
        VG        #PV #LV #SN Attr   VSize   VFree  
        fedora      1   2   0 wz--n-   9,51g      0 
        practica1   1   0   0 wz--n- 200,00m 200,00m
        ```



    * __LV :__ El Logical Volume o Volumen Logico, son "el producto final" del LVM (Logical Volume Manager). Son estos dispositivos los que usaremos para crear sistemas de ficheros, swap, discos para máquinas virtuales, etc… guir con la analogía del “disco duro virtual” que es el VG, los LVs serían las particiones. Con los que vamos a trabajar realmente. A diferencia de “sus primas” las particiones tradicionales, los LVs pueden crecer (mientras haya espacio en el VG) independientemente de la posición en la que estén, incluso expandiéndose por diferentes PVs. Un LV de 1G puede estar compuesto de 200MB procedentes de un disco duro, 400MB de un RAID software, y 400MB de una partición en un tercer dispositivo físico. El único requisito es que todo los PVs pertenezcan al mismo VG. Por supuesto, aunque posible, no parece una combinación con mucho sentido
    
        ___-lvcreate -L 250M -n videos multimedia ==> Esto creara un Volumen Logico (LV) dentro del Volumen de grupos (VG) el primer parametro indica el tamaño que debe tener, el segundo el nombre del sistema de ficheros dentro del "VG" y el tercero el destino (VG)___
        
        ___-lvs ==> muestra informacion del volumen logico (LV)___

        ```
        [root@Bryhan ~]# lvs
        LV    VG        Attr       LSize   Pool Origin Data%  Meta%  Move Log Cpy%Sync Convert
        root  fedora    -wi-ao----   8,51g                                                    
        swap  fedora    -wi-ao----   1,00g                                                    
        dades practica1 -wi-a----- 200,00m 
        ```

__2. Entorno de Practicas: Explicar como haremos la practica detalladamente (maquina virtual i añadir tres discos de 200M)__


* Para Crear un sistema de ficheros "xfs" en el VG, montar en /mnt y crear un fichero con "dd" de 180M
    * Crear sistemas de ficheros con :  
        ___<mkfs.xfs /dev/practica1/dades>___
    * Para montarlo en /mnt :  
        ___<mount /dev/practica1/dades /mnt>___
    * Para crear el fichero con "dd" de 180M:  
        ___-dd if=/dev/zero of=test.img bs=1k count=180000___

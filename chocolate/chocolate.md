nmap: 192.168.1.129

![image](https://github.com/user-attachments/assets/d17ebd57-216c-46bd-aece-555b03dab70c)

La pagina principal del puerto 80 es un apache2 sin mas, pero al hacer un gobuster

    gobuster dir -u http://192.168.1.129/ -w /usr/share/wordlists/seclists/Discovery/Web-Content/directory-list-2.3-medium.txt -x php,html,py 

nos dio: 

![image](https://github.com/user-attachments/assets/58916336-6df0-4c2c-8c17-b8557361e0d7)

y el mensaje de la pagina que dice:
>  Bob, comprueba que la limpieza se estÃ© ejecutando automÃ¡ticamente en el sistema

dándonos el usuario Bob
le hacemos fuerza bruta:

     hydra 192.168.1.129 ssh -l bob -p /usr/share/wordlists/rockyou.txt

nos da la contraseña

![image](https://github.com/user-attachments/assets/835ab1aa-09f6-4c86-bf5d-67a9c4b776b7)

entramos

![image](https://github.com/user-attachments/assets/952fd3d3-f5ec-4cdb-bcee-6c675408646e)

al entrar hay un .sh pero que no ayuda mucho y lo otro es que el usuario secretote que es el otro usuario esta por FTP y no se me ocurre otra cosa que hacer para escalar que intentar con otra fuerza bruta pero cambio el ssh por el ftp

![image](https://github.com/user-attachments/assets/70e08ee4-0b9c-4d17-b32f-269d796099be)


aquí se ve lo que contiene esa carpeta .sh y el ftp del otro usuario

la fuerza bruta: 

     hydra 192.168.1.129 ftp -l secretote -P /usr/share/wordlists/rockyou.txt

y conseguimos la contraseña

![image](https://github.com/user-attachments/assets/315398aa-6fb4-45cb-84f2-fe51abed3802)

ingresamos y al hacer un sudo -l nos muestra "man"

![image](https://github.com/user-attachments/assets/ec892b4c-1c16-463a-8ec9-3ff028905651)

buscamos como escalar por man y encontramos aqui: https://gtfobins.github.io/gtfobins/man/#sudo

seguimos los dos comandos que da

![image](https://github.com/user-attachments/assets/d9d1f758-6177-475b-b083-884af3fae0d6)

ejecutamos 

![image](https://github.com/user-attachments/assets/dee1dc23-ab3c-4259-96eb-cdaf51441633)

dentro del man man colocamos: 
> !/bin/sh

y seremos root y ahora podemos sacar la flag de root

la otra flag esta en la carpeta bob pero el permiso solo es para root, pero no podemos acceder a la carpeta bob con root

por esto: 

![image](https://github.com/user-attachments/assets/b02e1473-ed64-4afb-b798-468fc5f43cc2)

pero como somos root ahora, podemos modificar la carpeta /etc/passwd y así sacar poder ver el contenido

![image](https://github.com/user-attachments/assets/7478e77f-5edd-407d-8449-947e183831dc)

nos devolvemos al usuario secretote y escalamos a root solo con su root
y podremos leer el contenido de la flag

![image](https://github.com/user-attachments/assets/735950fb-8992-4073-a6f4-8cfb359fe438)

y ahora tendríamos la maquina completada. muchas gracias por leer. 

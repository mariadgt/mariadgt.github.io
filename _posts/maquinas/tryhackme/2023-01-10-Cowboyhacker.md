---
title: Cowboyhacker
date: 2023-01-10 14:00:00 PM
categories: [Máquinas, TryHackMe]
tags: [nmap, ssh, ftp, hydra]     # TAG names should always be lowercase
img_path: /assets/img/cowboyhacker
---
# COWBOYHACKER

Primeramente escaneamos los primeros 100 puertos que tiene la maquina para ir con mayor velocidad y en segundo plano, escaneamos los demás puertos de la maquina.

![Untitled](0.png)

![Untitled](1.png)

Después de esperar a que el primer comando termine podremos ver que nos saca los mismos puertos que el segundo comando.

Después de detectar los puerto de la maquina, lanzaremos el script de búsqueda de vulnerabilidades a los puertos. Como podemos ver en la captura, el puerto de ftp tiene vulnerabilidades:

```jsx
nmap -A -sV 10.10.46.19
```

![Untitled](2.png)

Es posible acceder al puerto ftp anónimamente, vamos a probarlo:

![Untitled](3.png)

Descargaremos los archivos encontrados y los abriremos y veremos su contenido.

![Untitled](4.png)

A continuación veremos el contenido de los dos archivos:

![Untitled](5.png)

![Untitled](6.png)

Después de sacar la lista de contraseñas, probaremos a entrar con fuerza bruta a ssh, con el usuario “lin”, que sacamos en el txt “task.txt”

![Untitled](7.png)

Accedemos a ssh con este usuario y contraseña y hacemos un ls para ver que nos da. Ya habríamos sacado la flag.

![Untitled](8.png)

Primero miraremos las aplicaciones que tienen permisos de administrador que tiene el usuario lin.

![Untitled](9.png)

Vemos que podemos ejecutar tar como administrador

Nos vamos al navegador y buscamos la pagina gtfobins, para coger el comando de escalada de privilegios, buscamos tar en el buscador y copiaremos el comando que nos sale.

![Untitled](10.png)

Una vez ejecutado el comando vemos que tenemos acceso como root, nos vamos a su carpeta principal y vemos el archivo que hay en esta:

![Untitled](11.png)

Realizado por:

[@danielsago](https://github.com/DanielSaGo)

[@mariadgt](https://github.com/mariadgt)
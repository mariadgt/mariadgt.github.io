---
title: Easy Peasy
date: 2022-12-15 14:00:00 PM
categories: [Máquinas, TryHackMe]
tags: [nmap, gobuster, john]     # TAG names should always be lowercase
img_path: /assets/img/easypeasy
---

# Easy peasy

Comenzamos ejecutando el comando para mostrar los puertos que tiene la maquina abiertos:

    sudo nmap -p- --max-rate=5000 -T5 -sS -Pn 10.10.149.213 -oN ports

![Untitled](0.png)

comprobamos la versión de lo que se esta corriendo dentro de cada puerto abierto:

    sudo nmap -p- -T5 -sR -Pn 10.10.149.213

![Untitled](1.png)

((para hacer todo lo anterior en un solo comando))

comprobamos los puertos y lo que se esta ejecutando en cada puerto:

    nmap -sC -sV -p- 10.10.149.213

![Untitled](2.png)

hemos encontrado que en el puerto 80 tenemos un http con un archivo robots.txt asi que comprobamos el contenido de este:

![Untitled](3.png)

usamos el gobuster para hacer fuerta bruta a los directorios:

    gobuster dir -u [http://10.10.149.213](http://10.10.149.213/) -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt

![Untitled](4.png)

Nos ha mostrado el directorio de hidden, nos vamos al navegador para ver que es:

![Untitled](5.png)

La inspeccionamos y no encontramos nada asi que volvemos a repetir el comando gobuster con el directorio hidden:

gobuster dir -u [http://10.10.149.213](http://10.10.149.213/)/hidden -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt

![Untitled](6.png)

Esta vez nos saca el subdirectorio whatever, volvemos al navegador para ver que nos muestra:

![Untitled](7.png)

lo inspeccionamos y nos muestra esto:

![Untitled](8.png)

Al ver los == del final suponemos que esta codificado en base 64 asi que vamos a descodificarlo:

![Untitled](9.png)

ejecutamos el comando gobuster para ver si hay más directorios:

![Untitled](10.png)

Ahora vamos a atacar el otro puerto con http que hemos descubierto, para ello nos vamos al navegador y buscamos la web que hay en ese puerto y le damos a inspeccionar:

![Untitled](11.png)

Accedemos al archivo robots.txt y nos aparece un hash, vamos a identificarlo, saber su tipo:

![Untitled](12.png)

Vemos que es un posible MD5 asi que nos vamos al navegador y lo desencriptamos:

![Untitled](13.png)



Probamos suerte y buscamos en la fuente de la pagina por hidden y nos aparece dicho hash:

![Untitled](14.png)

(en este caso tendriamos que probar muchos hashes para saber que tipo es pero sabemos que es base62)

Nos vamos al navegador para desencriptarlo:

![Untitled](15.png)

Nos vamos al navegador y probamos esa ruta, inspeccionamos la pagina:

![Untitled](16.png)

Copiamos ese hash en un archivo y se lo pasamos al john para descifrarlo junto con el archivo que nos ha proporcionado la pagina:

![Untitled](17.png)

Volviendo a la ultima inspeccion nos descargamos la imagen que tenemos.

La inspeccionamos con el comando steghide:

![Untitled](18.png)

Miramos lo que contiene el archivo que hemos sacado:

![Untitled](19.png)

Decodificamos el binario:

![Untitled](20.png)

Y probamos este usuario con esa contraseña para el ssh:

![Untitled](21.png)

Vemos que tenemos un archivo de usuario, le hacemos un cat:

![Untitled](22.png)

Tenemos una flag pero esta codificada, contamos las letras que hay desde la s hasta la f y sacamos como descodificarla:

![Untitled](23.png)

CONTINUARA...
# SELECTOR DE FONDO DE PANTALLA
Este script al ejecutarlo podrá seleccionar de un menú repetitivo diferentes opciones de gestión de usuarios como crear usuarios,borrarlos,ver los usuarios del sistema y demás.

Este script permite facilitar la administración y gestión de cuentas de usuarios y grupos. 

## **ATENCIÓN:**

Debes de ejecutarlo como root pero cargando su entorno porque sino puede que haya algun error y no funcione bien.


## **Prueba de script:**
![image](https://user-images.githubusercontent.com/74706392/116101711-5aa1b700-a6ae-11eb-8eea-d7d975799348.png)

![image](https://user-images.githubusercontent.com/74706392/116101787-6b522d00-a6ae-11eb-8163-41796eaa7f1c.png)

![image](https://user-images.githubusercontent.com/74706392/116101922-8a50bf00-a6ae-11eb-99b4-8ad8a88a238c.png)

![image](https://user-images.githubusercontent.com/74706392/116102041-a18fac80-a6ae-11eb-9cf2-eba5550b359a.png)

![image](https://user-images.githubusercontent.com/74706392/116101503-2af2af00-a6ae-11eb-98af-82f81a327fcc.png)

![image](https://user-images.githubusercontent.com/74706392/116101592-3d6ce880-a6ae-11eb-90ec-bd5f5300dbd7.png)



## CÓDIGO

```
#!/bin/bash
# Author: Víctor Sánchez
# Versión: 1.0
#Funciones
menu () {
    echo "*******************************************"
    echo "Menu repetitivo de Gestion de usuarios"
    echo "*******************************************"
    echo
    echo "1.-Crear usuario"
    echo "2.-Borrar usuario"
    echo "3.-Cambiar contraseña"
    echo "4.-Ver usuarios del sistema"
    echo "5.-Asignar Usuario a grupo"
    echo "6.-Salir"
    echo
    read -p "Introduce una opción: " opcion
    while [ "$opcion" != "6" ]
    do
    case $opcion in 
    
    1) CreaUsuario;;
    2) BorraUsuario;;
    3) CambioContra;;
    4) MostrarUsuario;;
    5) UsuarioGrupo;;
    *) echo "Opcion incorrecta";;
    esac
    read -p "Introducir cualquier tecla" A
    clear
    menu
    done
}

CreaUsuario () {
    read -p "Nombre de usuario que quiere crear: " usuario
    resultado=$(cat /etc/passwd |grep -w "$usuario")
    if [ -z "$resultado" ]
    then
        adduser "$usuario"
        echo "Usuario creado"
    else
        echo "El usuario $usuario ya existe "
    fi
    
}

BorraUsuario () {
    read -p "Nombre de usuario que quiere borrar: " usuario
    resultado=$(cat /etc/passwd |grep -w "$usuario")
    if [ -z "$resultado" ]
    then
        echo "El usuario no existe o ya ha sido eliminado"
    else
        userdel "$usuario"
        echo "Usuario borrado"
    fi
}

CambioContra () {
    read -p "Nombre de usuario que quiere cambiar la contraseña: " usuario
    resultado=$(cat /etc/passwd |grep -w "$usuario")
    if [ -z "$resultado" ]
    then
        echo "El usuario no existe"
    else
        passwd "$usuario"
    fi
}

MostrarUsuario () {
    getent passwd
}

UsuarioGrupo () {
    read -p "Usuario: " usuario
    read -p "Grupo donde agisnar al usuario: " grupo
    resultado=$(cat /etc/passwd |grep -w "$usuario")
    resultado2=$(cat /etc/group |grep -w "$grupo")
    if [ -z "$resultado" ]
    then
        echo "El usuario no existe"
        read -p "¿Quiere crearlo?(si o no)" respuesta
        if [ "$respuesta" = "si" ]
            then
                adduser "$usuario"
            else
                echo
                return 1
            fi
    fi
    if [ -z "$resultado2" ]
    then
        echo "El grupo no existe"
        read -p "¿Quiere crearlo?(si o no)" respuesta
        if [ "$respuesta" = "si" ]
            then
                addgroup "$grupo"
            else
                echo
                return 1
            fi
    fi
    adduser "$usuario" "$grupo"

}

ComprobarRoot ()
{
    if [ "$(whoami)" != "root" ]
    then
        echo "No eres el root"
        exit 1
    fi
}

#Bloque principal
ComprobarRoot
menu


```

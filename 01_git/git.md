# Guía de uso básica de [git](https://git-scm.com/). (OS: Arch Linux)

# Índice 

- [0. Instalación y configuración de git](#0-instalación-y-configuración-de-git)
- [1. Inicialización del proyecto](#1-inicialización-del-proyecto)
- [2. Flujo básico de trabajo](#2-flujo-básico-de-trabajo)
- [3. Conectar `git` con `GitHub`](#3-conectar-git-con-github)
- [4. Añadir repositorios locales a GitHub.](#4-añadir-repositorios-locales-a-github)

# 0. Instalación y configuración de git

#### · Arch Linux: 
Simplemente instalar del repositorio oficial: 
`sudo pacman -S git`

#### · Configuración básica: 
```
$ git config --global user.name "Your Name Comes Here"
$ git config --global user.email you@yourdomain.example.com
```

# 1. Inicialización del proyecto
Muévete en una terminal al repositorio en el que quieras usar `git`.
Una vez ahí, inicializa `git` con el siguiente comando: 
```
$ git init
```

# 2. Flujo básico de trabajo

### 2.1. Conceptos previos

Para 'tomar una instantánea' del proyecto hay que hacerlo desde la fase de `stage`. La 'instantánea' se realiza al ejecutar un `commit` de los archivos y carpetas que se encuentren en la fase de `stage`. Para añadir archivos y carpetas a esta fase se hará ejecutando el comando `add`. 

### 2.2. Flujo estándar de trabajo

1. Comprobar el estado de los archivos/carpetas en fase `stage`: 
```
$ git status
```
La consola nos devolverá que archivos/carpetas no están añadidos. 

2. Añadir archivos a `stage`:
```
$ git add [ruta del archivo a añadir]
```
3. Registrar  la 'instantánea' del proyecto: 
```
$ git commit -m "[Breve mensaje que describa el commit]"
```
La bandera `-m` indica que pasaremos la descripción del `commit` desde el propio comando en vez de forzar la apertura de un editor de texto.

# 3. Conectar `git` con `GitHub`

Nota: siempre será recomendable leer la documentación oficial de [GitHub](https://docs.github.com/es/authentication/connecting-to-github-with-ssh/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent)

1. Instalar (y/o actualizar) `ssh`: 
```
$ sudo pacman -S openssh
```
2. Ejecuta el siguiente comando para generar la llave `ssh`: 
```
$ ssh-keygen -t ed25519 -C "your_email@example.com"
```
Te pedirá que introduzcas parámetros opcionales. Si no tienes mas claves `ssh` puedes puedes omitir estos parámetros.

3. Inicia el agente SSH en segundo plano:
```
$ eval "$(ssh-agent -s)"
> Agent pid 59566
```
4. Agrega tu llave privada `ssh` al `ssh-agent`:
```
$ ssh-add ~/.ssh/id_ed25519
```

5. Incorpora la clave `ssh` a `GitHub`:
    5.1. Ve a [Ajustes de GitHub](https://github.com/settings/keys).
    5.2. Click en ***New SSH key***
    5.3. Añade un título descriptivo del equipo que va a enlazar.
    5.4. `Key type` --> `Authentication Key`
    5.5. En el campo ***Key*** pega la clave pública.
    Para obtener la clave publica:
    ```
    $ cat ~/.ssh/id_ed25519.pub
    ```
Después de todo esto es probable que te pida una verificación de identidad. 

6. Prueba de conexión.
Para comprobar que lo hemos hecho todo bien deberemos realizar la siguiente comprobación: 
```
$ ssh -T git@github.com
```
El comando te dará un 'error'. Es normal, comprueba que la clave que te devuelve la consola coincide con algunas de las que aparecen [aquí](https://docs.github.com/es/authentication/keeping-your-account-and-data-secure/githubs-ssh-key-fingerprints). Si alguna de estas claves coinciden simplemente escribe `yes`. 
Si el mensaje que aparece es: 
```
> Hi USERNAME! You've successfully authenticated, but GitHub does not provide shell access.
```
significa que todo ha salido bien.


# 4. Añadir repositorios locales a GitHub.
Si tienes un repositorio local que quieres añadir a un repositorio de GitHub deberás: 
1. Añadir el repositorio remoto de GitHub: 
```
$ git remote add origin git@github.com:[user-name]/[repo-name]
```

2. 'Pushear' los archivos locales al repositorio remoto: 
```
$ git push -u origin [nombre de la rama]
```
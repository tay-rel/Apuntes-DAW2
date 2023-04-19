# Instalación git

Se debe comprobar primero si lo tenemos instalado en nuestro sistema operativo.

```bash
git --version
```

Si no sale ninguna vesion lo más probable es que no este instalado.Asi que se realiza los siguientes pasos:

```bash
sudo apt install git
```


# Configuración de Git

Git trae una herramienta llamada `git config`, que te permite obtener y establecer variables de configuración que controlan el aspecto y funcionamiento de Git.

### Tu Identidad

```bash
$ git config --global user.name "John Doe"
$ git config --global user.email johndoe@example.com
```

Podemos ver todos los elementos de configuración creados escribiendo lo siguiente:

```bash
git config --list
```

Si utilizas el transporte SSH para conectarte a remotas, es posible que tengas una clave sin frase de contraseña, lo que te permite transferir datos de forma segura sin escribir tu nombre de usuario y contraseña. Sin embargo, esto no es posible con los protocolos HTTP: **cada conexión necesita un nombre de usuario y una contraseña**. Esto se hace aún más difícil para los sistemas con autenticación de dos factores, donde el token que utilizas como contraseña se genera aleatoriamente y es impronunciable.

Esto significa que hasta que no cambies tu contraseña para el host Git, no tendrás que volver a escribir tus credenciales.

Para esto usamos el siguiente comando :

```bash
git config --global credential.helper store
```

Podemos establecer una rama por defecto se asegura de que cualquier nuevo repositorio que se cree tenga la que queremos por defecto en este caso será la rama `main`.

```
git config --global init.defaultBranch main
```

# Comandos Git

- Iniciar proyecto, al iniciarlo se crea el siguiente fichero `.git`:  

`git init`

- Añade a una rama master/main:

`git remote add origin <rama>`

* Añade cambios a local :

`git add  .`

- Agrega comentario:

`git commit -m "mi primer commit"`

- Ver ficheros añadidos en el git add  

`git status`

## Moverse entre Ramas

- Crear una nueva rama local conservando cambios actuales  

`git checkout -b <rama>`

- Cambiar de rama y te pociona en ella.  

`git checkout <rama>`

- Crea una nueva rama a partir de un commit

`git branch <nueva rama> <commit>`

- Borra una rama local  

`git branch -d <rama>`

- Cambia el nombre de una rama:

`git branch -m main`

- Lista las ramas locales que tenemos:

`git branch -a`

`git branch --list`


## Modificaciones en los cambios de un proyecto

- Muestra los cambio que se ha echo:

`git chekcout`

- Vuelve al ultimo commit:

`git checkout -- .`

- Sube a la nube:

Normal: `git push origin <rama>`

Forzando: `git push -f -u origin <rama>`

- Baja de la nube los cambios:

`git pull`

- Borrar ultimo commit:

`git reset --hard HEAD~1` *por eliminar esa confirmación de la sucursal local*

`git push origin HEAD --force` *ambos comandos deben ser ejecutados. Para eliminar de la rama*

#### *Cambiar de rama  repositorio*

Antes de nada se debe verificar en que rama y repositorio estamos y depues proseguir con lo siguiente , para trabajar a partir de esa rama y repositorio.

- Eliminar la rama antigua

`git remote remove origin`

- Añadir tu rama

`git remote add origin https://gitlab.com/otro_usuario/otro_repositorio.git`

- Comprobar rama y repositorio 

`git log`

## Clonar

- Clonar un repositorio:

`git clone [https://github.com/tay-rel/DAW2.git](https://github.com/user/repo.git)`

- Clonar una rama específica:

`git clone -b <rama> [https://github.com/tay-rel/DAW2.git](https://github.com/user/repo.git)`

## Bibliografia

[Git](https://git-scm.com/book/es/v2/Inicio---Sobre-el-Control-de-Versiones-Configurando-Git-por-primera-vez)
[Borrar commits]([https://qastack.mx/programming/22620393/various-ways-to-remove-local-git-changes](https://qastack.mx/programming/22620393/various-ways-to-remove-local-git-changes))
[Ramas](https://es.stackoverflow.com/questions/191716/cambiar-de-repositorio-remoto-en-un-repositorio-local-con-git)

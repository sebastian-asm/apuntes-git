# Apuntes de GIT

Simulador: [https://git-school.github.io/visualizing-git/](https://git-school.github.io/visualizing-git/)

En GIT las carpetas de proyectos son conocidos como _repositorios_.

Los 3 estados de GIT:

1. **Working Directory (modified)**: es nuestro directorio de trabajo
2. **Staging Area (staged)**: son los cambios que están próximos a hacer guardados en una confirmación (commit)
3. **Local Repository (committed)**: son los cambios ya confirmados y guardados en la línea de tiempo

_HEAD_ será un puntero que nos indicará en que punto del historial nos encontramos.

Un _branch_ (rama) es una línea de tiempo de commits.

En un _merge_ (fusión) de ramas pueden ocurrir los siguientes panoramas:

1. **Fast-forward**: se dispara cuando GIT detecta que no hay ningún cambio en la rama principal, y los nuevos cambios se pueden fusionar sin conflictos.
2. **Uniones automáticas**: cuando GIT detecta que en la rama principal hubo algún cambio y que las ramas secundarias desconocen, pero cuando se intenta hacer la fusión, GIT lo hace automaticamente sin problemas (cuando no existe conflicto, o sea, no se modificaron las mismas líneas de código).
3. **Manual**: es cuando GIT no puede hacer la fusión de forma automática debido a que se tocaron las mismas líneas, cuando esto se soluciona se crea un nuevo commit llamado "merge commit", despúes de esto se puede seguir trabajando sin problemas. En VSCode de forma interactiva nos preguntará que queremos hacer ante un conflicto:

   - _Aceptar el cambio actual_: mantendrá mi código tal cual e ignorará los que vienen
   - _Aceptar el cambio entrante_: ignorará mis cambios y aceptará los que vienen
   - _Aceptar ambos cambios_

Las _tags_ (etiquetas) son una referencia a un commit específico (haciendo referencia a todo el estado del repositorio en ese momento del tiempo), utilizados principalmente para marcar versiones o release del código.

Por defecto GIT no hace seguimientos de carpetas vacías, pero para indicarle que sí lo haga, dentro de la carpeta se agrega el archivo _.gitkeep_.

Para evitar que GIT haga seguimientos de archivos y/o carpetas, se especifican mediante un listado dentro de un archivo llamado _.gitignore_ en el root del repositorio.

Los _stash_ son como unas cajas que nos permiten guardar todos los cambios temporales sin perderlos, para dejar la rama tal cual como se encontraba en su último commit.

Un _rebase_ nos serviría para las siguientes situaciones: ordenar, corregir mensajes, unir y separa commits. Se recomienda usar el rebase en forma local para evitar conflictos en caso que el repositorio este siendo compartido.

Hacer un _pull request_ (Github) es cuando se "desprende" una rama de otra en algún momento de la línea de tiempo, luego se puede volver a hacer un merge pero antes se realiza un análisis de lo que se quiere fusionar.

## Comandos

### Configuración

- `git config`: agregar configuraciones a GIT

  - --global user.email {email}: configurar un email a nivel global
  - --global user.name {usuario}: configurar un nombre a nivel global
  - --global init.defaultBranch {nombre}: establecer el nombre de la rama principal
  - --global alias.{nombre} "{comando} {banderas}": crear alias abrevido de algún comando
  - --global -e: ver la configuración global en el editor
  - --global --list: ver la configuración en la terminal
  - core.autocrlf true: evitar problemas con el CRLF (interpretación de un carácter)

### Generales

- `git --version | -v`: conocer la versión de GIT

- `git help {comando} | {comando} --help`: obtener ayuda acerca de un comando

  - -h: muestra ayuda reducida

- `git init`: inicializar un repositorio local

### Repositorio

- `git status`: conocer el estado actual del repositorio

  - -s: mostrar el estado de forma abreviada
  - -b: mostrar la rama local y remota actual

- `git log`: ver el historial de los commits (confirmaciones)

  - --oneline: mostrar el log de forma reducida
  - --stat: muestra cuales fueron las líneas que cambiaron
  - --all: mostrar todos los commits y ramas
  - --graph: representación gráfica de la línea de tiempo de los commits
  - --grep={consulta} -i: buscar en los mensajes de los commits sin importar mayúsculas o minúsculas
  - -p: mostrar las modificaciones de archivos en cada commit
  - -{limite}: indica el límite de commits para listar
  - -- {archivo(s)}: muestra los commit de un archivo

- `git shortlog`: listar los commits por usuario

- `git add {archivo(s)} | add .`: agregar archivos al Staging Area

  - \*.html: agregará al stage todos los .html del root
  - js/\*.js: agregará al stage todos los .js de la carpeta js
  - css/: agregará todos los archivos y subdirectorios de la carpeta css al stage
  - -p: mostrar los cambios que se guardarán en el stage

- `git commit -m {mensaje}`: realizar una confirmación (registro histórico) del estado actual del repositorio con un mensaje

  - -am {mensaje}: fusión entre el comando git add y git commit
  - --amend -m {mensaje}: modificar el mensaje del último commit

- `git diff`: comparar los cambios de archivos con las modificaciones no guardadas en el stage

  - --staged: comparar con los cambios agregados al stage
  - {commit} {commit}: hacer comparaciones de cambios entre commits
  - {commit} {commit} {archivo}: ver diferencias de un archivo entre un commit y otro

- `git reflog`: lista de todos los movimientos registrados del repositorio

- `git tag`: listar todos los tags

  - {nombre}: crea un tag en el punto del tiempo actual (se recomienda llevar nombres versionados semánticos, por ejemplo, v1.0.0)
  - -d {nombre}: eliminar tag
  - -a {nombre} -m {mensaje}: permite agregar un mensaje descritivo al tag creado
  - -a {nombre} {hash_commit} -m {mensaje}: agrega el tag en el commit indicado

- `git checkout {rama | hash_commit | tag}`: nos mueve a una rama, commit o tag

  - -- {archivo} | -- .: reconstruir el repositorio y/o archivo a como estaba en el último commit (solo a los que se les estaba haciendo seguimiento)
  - -b {rama}: crea una nueva rama y nos mueve a ella
  - {hash_commit} {archivo}: recuperar un archivo tal cual como se encontraba en un determinado commit

- `git reset {archivo} | reset .`: eliminar un archivo del Staging Area

  - --soft HEAD^ | {hash_commit} | {hash_commit}^{número_posición_commit}: guardar e incorporar cambios en el último commit (soft mantiene los cambios y ^ apunta al último commit antes del HEAD)
  - --mixed {hash_commit}: vuelve al punto del tiempo sacando los archivos del stage y dejando listo para volver a guardar en un nuevo commit
  - --hard {hash_commit}: se dejará al repositorio tal cual a como estaba en ese punto del tiempo destruyendo todo lo demás
  - --hard HEAD^: deshacer el útlimo commit con todos sus cambios

- `git stash`: guardar los cambios recientes de forma temporal (WIP: Working In Progress)

  - list: listar los stash
  - list --stat: listar con breve resumen de cambios guardados
  - pop: recuperar el último stash y lo elimina
  - clear: borrar todos los stash en memoria
  - apply {posición}: aplicará los cambios guardados del stash especificado
  - drop {posición}: eliminar un stash, si no se indica la posición eliminar el último
  - show {posición}: muestra los cambio guardados del stash
  - save {mensaje}: guardar un stash indicando un mensaje de referencia

- `git mv {archivo} {nuevo_nombre}`: renombrar un archivo

- `git rm {archivo}`: eliminar archivo

  - --cached {archivo}: eliminar del stage

- `git restore {archivo} | restore .`: permite recuperar el archivo tal cual como estaba en su último commit (similar a git checkout -- {archivo})

  - --staged {archivo}: elimina los cambios que hayan pasado al stage
  - --staged --worktree {archivo}: elimina los cambios del stage y del working directory
  - --source={hash_commit | tag} {archivo}: recupera el archivo tal cual se encontraba en un determinado commit o tag

- `git grep {expresión | regex}`: buscar una expresión en todo nuestro código, indicando en que archivo se encuentra

  - {expresión | regex} -- {ruta}: buscará coincidencias en la ruta indicada
  - -i: buscar sin importar mayúsculas o minúsculas
  - -w: buscar la expresión exacta
  - -n: muestra el número de línea en donde se encuentra la búsqueda

### Ramas

- `git branch`: listar ramas

  - {rama}: crea una nueva rama
  - -m {rama} {nuevo_nombre} | -m {nuevo_nombre}: modificar el nombre de una rama | modificar el nombre estando en la rama
  - -d | -D {rama}: eliminar rama | eliminar rama no importa si tiene cambios por guardar
  - -a: mostrar todas las ramas incluidas las remotas

- `git merge {rama_importada}`: permite hacer la fusión de ramas en un nuevo commit, trayendo la rama que contiene lo nuevos cambios

- `git switch {rama}`: nos mueve de rama

  - -c: crea una nueva rama y nos mueve a ella
  - --discard-changes {rama}: descarta todos los cambios al moverse de ramas
  - --orphan {rama}: crear una rama vacía, sin puntero e independiente (huerfana), especialmente para documentación
  - -: volver el HEAD al último commit de la rama
  - -d {hash_commit | tag}: moverse a un commit o tag

- `git rebase {rama}`: a diferencia de merge, el rebase no crea un nuevo commit sino que "aplana" ambas ramas unificando los commits en una sola, esto hace que los commit que se trajeron sean eliminados y pasen a tener un nuevo hash en la rama a donde se aplicaron (se sobreescribe el historial). También se puede decir que sirve para actualizar el punto de separación de una rama

  - -i HEAD~{cantidad_commits}: modo interactivo para tomar las cantidad de commits especificada despúes del HEAD (HEAD apunta al último commit)

    - s | squash: fusionar commits
    - r | reword: cambiar el mensaje de un commit
    - e | edit: editar los archivos de un commit o separarlos en varios (una vez finalizado se tiene que correr el comando git rebase --continue)

### Repositorio Remoto

Flujo de trabajo recomandado para Github: [https://blog.mergify.com/understanding-the-github-pull-request-workflow/](https://blog.mergify.com/understanding-the-github-pull-request-workflow/)

- `git remote -v`: ver todas las conexiones a repositorios remotos

  - add {alias_remoto} {url}: agregar una conexión remota a nuestro repositorio local (por convención, se utiliza el alias **upstream** para indicar un remoto del cual solo se traerá información)
  - rename {alias_remoto} {nuevo_alias}: modificar el alias
  - remove {alias_remoto}: eliminar una conexión
  - prune {alias_remoto}: hace "limpieza" de las ramas inexistentes

- `git push {alias_remoto} {rama_local}`: subir los cambios del repositorio local al remoto

  - -u: establecer una conexión por defecto
  - --all: sube todas las ramas y commits locales al remoto
  - --tags: sube todas las etiquetas
  - {alias_remoto} :{rama_remota}: eliminar rama en caso de seguir existiendo en el remoto

- `git pull {alias_remoto} {rama_remota}`: trae los cambios del respositorio remoto al local, cuando se utiliza la bandera -u en el push no es necesario especificar el alias ni la rama del remoto

  - --all: traer todas las ramas y sus commits del remoto al local

- `git clone`: permite clonar (su rama principal y commits) desde un repositorio remoto

- `git fetch`: permite **solo** actualizar las referencias (ramas y commits) entre el local y remoto, a menos que hagamos un git pull, el HEAD **no** apuntará al último commit que se haya creado en el remoto. Se puede considerar que es solo un comando informativo sobre los cambios y lo que podría ocurrir

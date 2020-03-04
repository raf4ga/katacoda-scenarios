Una vez empezamos a utilizar Ansible pronto **nos acabaremos dando cuenta de que esto de crear un playbook no es especialmente independiente**, es decir, no podría coger una parte que, perfectamente, podría categorizar como necesaria para conseguir un objetivo 'X' y, luego, llevármela para usar solo esa en combinación con otras distintas. 
 
Un ejemplo clave es una serie de tareas para instalar y configurar un apache.  
 
Generalmente esta labor tendría dos o tres tareas (instalar apache, configurar apache y reiniciarlo para aplicar la configuración, por ejemplo) y con ello ya podría decir, perfectamente, que cuento con un apache instalado en mi equipo.  
 
Bien, pues si quisiera tener esto aislado para utilizarlo en cualquier lado... **¡Es posible!** 
 
# Los roles, nuestros mejores amigos 
**Un rol es**, básicamente, **un *playbook* de Ansible que cuenta con una serie de recursos y que es totalmente trasladable a otro entorno, llamándolo desde un *playbook* general que delega en él**.  
 
Se encuentra aislado de otros roles, por lo que **solo podrá ver aquello que encuentre a su "*nivel*"** y, además de lo que ya permite un *playbook* en si, extiende su utilidad con elementos como ficheros propios, variables constantes y no tan constantes y, además, capacidad para utilizar plantillas para generar diversos ficheros. 
 
En esta etapa de este pequeño curso de Ansible aprenderemos cómo generar uno muy genérico y hacer pruebas con él. 
 
Como ya tenemos una base que viene desde la tarea anterior... ¡vamos a aprovecharla! 
 
- En primer lugar, **generamos nuestro directorio de roles**, al mismo nivel que el directorio de inventory y el del playbook: `mkdir roles`{{execute}} 
- Nos colocamos dentro de este nuevo directorio y **creamos uno nuevo**: `cd roles`{{execute}} & `mkdir essentials-installer`{{execute}} 
- **Creamos un directorio básico** (hay más, pero por simplificar utilizaremos éste, que es el único realmente necesario): `cd essentials-installer`{{execute}} & `mkdir tasks`{{execute}} 
- **Y otro más para guardar algún fichero que queramos copiar**: `mkdir files`{{execute}}
 
# La base de todo rol: una necesidad, una idea... 
Bien, **ya tenemos un árbol de directorios muy base para crear nuestro primer rol**. Antes de empezar a escribir el fichero *.yml* que contará con las distintas tareas a ejecutarse, **vamos a poblar un poco nuestro directorio "*files*"**.

**Este directorio se utiliza en los roles para guardar ficheros que queramos copiar en una máquina**. Puede tener subdirectorios y gracias a la potencia de los módulos de Ansible, podremos o bien copiar una serie de ellos o directamente llevarnos un directorio entero con sus subdirectorios.

¡Así de simple!

Primero, vamos a crearnos un fichero simple, al que llamaremos "ansible-test.txt". Para simplificarlo, vamos a rellenarlo directamente a través de un comando sin tener que editar ningún fichero entre medio: `echo "Ansible es la leche" >> files/ansible-test.txt`{{execute}}

Y de paso, para comprobar cómo funciona con subdirectorios, vamos a crearnos uno nuevo con `mkdir files/ansible-hater`{{execute}} y añadirle el siguiente fichero `echo "Pues a mi me gusta mas hacerlo todo a mano..." >> files/ansible-hater/ansible-hate.txt`{{execute}}
 
Una vez hecho esto, **podemos empezar con nuestro rol, que tendrá las siguientes tareas**: 
- Instalación de 'nano', un editor más simple para UNIX (a través del módulo '*yum*') 
- Creación de un usuario base y un grupo adicional (a través del módulo '*user*' y el módulo '*group*'). 
- Copiado de ficheros a su directorio home (a través del módulo '*copy*'). 


# Creando y rellenando el fichero de tareas del rol
Antes de empezar a rellenar el fichero de tareas, vamos a tener que crearlo. El fichero que vamos a colocar dentro del directorio "tasks" lo vamos a llamar `main.yml`. Muy atento a este nombre, porque se va a repetir: es el nombre que ha de tener el fichero a utilizar tanto en el directorio de tasks, como en el de vars, como en el de default y así con unos cuantos más.

Si bien no hemos mencionado aún estos directorios, te irán sonando conforme más utilices Ansible, ya que son muy utilizados en los roles.

Para crearlo en el punto exacto, podemos servirnos de este comando: `touch tasks/main.yml`{{execute}}
 
Una vez hecho esto, comenzaremos con la tarea que instala el editor '**nano**': 
```yaml 
- name: Install nano editor 
    apt:  
      name: nano
      state: latest 
``` 
 
Seguida de la que crea el grupo: 
```yaml 
- name: Create group 'ansible-pro'
    group: 
      name: ansible-pro
      gid: 1337
      state: present
```

Y después el que crea el usuario...

```yaml
- name: Creates user ansible-master 
    user: 
      name: ansible-master
      shell: /bin/bash 
      groups: ansible-pro 
      append: yes
```

Y por último copiamos tanto el fichero...

```yaml
- name: Copies test file to home directory
    copy:
      src: ansible-test.txt
      dest: /home/ansible-master/
```

Y el directorio de ansible-hater completo...

```yaml
- name: Copies test folder to home directory
    copy:
      src: ansible-hater
      dest: /home/ansible-master/
```

# Llamando al Rol desde el Playbook
Por último, tendremos que volver a editar el playbook.yml para decirle que ejecute el nuevo rol que acabamos de crear:

```yaml
---
- hosts: localhost
    roles:
      - essentials-installer
```

Y listo, ahora ya podemos ejecutar nuestro rol desde un playbook. 

# Ejecutando nuestro primer Rol de Ansible
¿Os acordáis de cómo se ejecutaba Ansible? Venga, no era tan difícil... ¡Primero tenemos que localizarnos en el directorio de ansible! `cd /home/scrapbook/tutorial/ansible/`{{execute}} 

Y luego ejecutamos nuestro comando de ansible para lanzar el playbook `ansible-playbook playbook.yml`{{execute}} ...

Veremos cómo hemos conseguido hacer todo, a través de diversos comandos (lamentablemente, al estar en otra localización, no podremos usar nuestro IDE...):

- `cat /etc/passwd | grep ansible-master`{{execute}} (el usuario)
- `cat /etc/group | grep ansible-pro`{{execute}} (el grupo)
- `ls -l /home/ansible-master`{{execute}} (el fichero ansible-test.txt)
- `cat /home/ansible-master/ansible-test.txt`{{execute}} (el contenido de dicho fichero)
- `ls -l /home/ansible-master/ansible-hater`{{execute}} (lo que había dentro del directorio de ansible-hater)
- `cat /home/ansible-master/ansible-hater/ansible-hate.txt`{{execute}} (el contenido del fichero)

**A partir de aquí las historias se repiten**, aunque aumentando el nivel de complejidad haciendo tareas algo más elaboradas, utilizando variables de entorno o variables propias del rol que podemos (o no) modificar, etc...

Con todo esto ya tendríamos una base bastante firme sobre cómo crear nuestro propio rol de Ansible, y con ello bien podríamos empezar a trastear con total normalidad... 

**¡Enhorabuena! ¡Eres un crack!**

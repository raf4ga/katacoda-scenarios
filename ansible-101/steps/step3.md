Tenemos nuestra carpeta base, y en principio, **poco más vamos a necesitar para crear nuestro primer playbook**. 
 
Aún así, **es recomendable crearnos nuestro propio inventario para nuestro playbook de Ansible**: muchas veces tendremos playbooks que no tienen que ver entre ellos y la mejor manera de diferenciarlos es hacer que cada uno pueda única y exclusivamente atacar a las máquinas que sean necesarias.  
 
Para empezar, vamos a meternos dentro del directorio de Ansible (si no lo hemos hecho ya): `cd ansible`{{execute}} 
 
#### ¿Qué es un inventario? 
Especificado bajo el nombre '*inventory*', un inventario viene a ser un **fichero (o una serie de ellos) que especifican los hosts a los que va a atacar nuestro playbook de Ansible**. Como adelanta la frase anterior, no tiene por qué ser uno solo, y cada playbook puede utilizar cualquiera de ellos.  
 
#### Creando nuestro primer inventory 
Los ficheros de inventario suelen ubicarse bajo una carpeta que está en al mismo nivel que el *Playbook*: 'inventory'. Esta carpeta suele contener un fichero llamado "*hosts*", aunque el nombre puede ser el que queramos, sin ningún tipo de problema (solo que de tener un nombre distinto tendremos que referenciarlo al llamar al comando del playbook). 
 
Para ello vamos a utilizar un comando ya conocido: `mkdir inventory`{{execute}} 
 
Y bajo él vamos a crear un inventory simple... 
 
`echo "[localhost]" > inventory/hosts`{{execute}} 
`echo localhost >> inventory/hosts`{{execute}} 
 
Listo, en nuestro IDE ya deberíamos ver cómo, bajo la carpeta *ansible/inventory*, ya tenemos nuestro fichero hosts rellenado. 
 
#### Ansible y el lenguaje YAML 
Ya tenemos una **base clara, donde empezar a ejecutar nuestro Playbook aislado**. Ahora lo único que queda es crear nuestro playbook y rellenarlo. 
 
Para ello, vamos a crear el fichero tal cual: `touch playbook.yml`{{execute}} 
 
Como podemos ver, **su extensión es "*.yml*"**. Esto significa que es un fichero del tipo "*yaml*", formato que se utiliza para serializar datos de manera legible y que está **inspirado en lenguajes como *C*, *Python*, *XML* o *Perl***.  
 
Es **bastante utilizado**, no solo por Ansible, sino que también por frameworks como *Laravel* o la solución **PaaS** de *Red Hat: Openshift*.  
 
Es muy fácil de aprender y la única problemática que puede ofrecer es la de que necesita de una indentación específica o de lo contrario no va a funcionar. 
 
El caso, es que **para probar un primer playbook.yml lo más simple posible, vamos a ejecutar directamente desde la shell**. Generalmente esto no es recomendable, ya que lanzar el módulo de Ansible de Shell significa estar ejecutando código de manera nativa en la máquina, pero como estamos de pruebas y queremos algo sencillo, por ahora bastará.  
 
En caso de que vayáis a utilizar este recurso como aprendizaje para un uso más profesional de Ansible, recordadlo bien: **no se recomienda utilizar el módulo Shell a no ser que sea _ESTRICTAMENTE_ necesario** 

#### ¿Qué es un Playbook?

**Ansible funciona a través del uso de unos ficheros donde se le dice lo que hay que ejecutar**. Podemos pensar en un *playbook* como una lista con todas las tareas que hay que realizar para conseguir lo que queremos. 

Los *playbooks* **tienen varias formas distintas de utilizarse**: existen configuraciones donde se listan las tareas tal cual, así como *playbooks* que llaman a un *rol*, que es el que tiene la lista de tareas, junto a otros ficheros; **también existen *playbooks* que llaman a otros ficheros *.yml* donde se encuentran las tareas** y si vamos más allá, ***playbooks* que llaman a otros *.yml* que llaman a su vez a un rol dentro de ellos, cuyo fichero *.yml* interior llama a otros ficheros *.yml* donde se encuentran las tareas...**

Eso ya es crear una *Matrioshka* demasiado compleja para lo que queremos, pero deja ver que Ansible es altamente configurable, siempre y cuando nos quedemos dentro de su cerco.

Con lo que tenemos que quedarnos es que es un fichero de extension *.yml* y es donde se ponen las distintas tareas, siguiendo cierto convenio que veremos a continuación...
 
#### Rellenando nuestro playbook.yml 
 
Vamos a rellenar, entonces, nuestro ***playbook.yml***. Esta es la configuración recomendada: 
 
```yaml 
--- 
- hosts: localhost 
    tasks: 
    - name: Say 'Hello World' through echo 
      shell: echo "Hello World" 
 
    - name: Store 'Hello' in a file 
      shell: echo "Hello" >> /home/scrapbook/tutorial/ansible-output.txt 
``` 
 
**Para comprender mejor el código:** contamos con 2 tareas principales a ejecutarse en este playbook. 
 
La primera de ellas simplemente lanza un `echo "Hello World"` como si lo escribiéramos tal cual en nuestra terminal (tiene el mismo efecto). 
 
La segunda de ellas hace exactamente lo mismo, pero esta vez simplemente hace un `echo "Hello"` pero en vez de imprimirlo por pantalla, se encarga de dejarlo caer en un fichero que se llamará 'ansible-output.txt' en nuestro directorio raíz. 
 
Para ejecutarlo simplemente tendremos que hacer lo siguiente, situándonos dentro del directorio de '*ansible*': `ansible-playbook playbook.yml`{{execute}} 
 
Y si hacemos el siguiente comando... `cat /home/scrapbook/tutorial/ansible-output.txt`{{execute}} veremos que, efectivamente, vemos que se ha escrito "*Hello*" dentro del fichero... ¡todo de un botonazo! 

Pero, ¿qué ocurre con la otra orden? La de `echo "Hello World"`...

Pues bien, se ha ejecutado, esto es cierto, pero no la hemos visto en nuestra ejecución de Ansible. Esto es así porque se ha ejecutado a través del módulo de la Shell, pero tal y como hemos ejecutado no vamos a poder ver el resultado.

Si queremos ver más, tendremos que aplicarle niveles de "verbosidad" a través de la opción "-v"... Así: `ansible-playbook -v playbook.yml`{{execute}}

O si tenemos un día muy "verboso"... `ansible-playbook -vvv playbook.yml`{{execute}} (depende de cuánto quieras ver).

Si lo hacemos, veremos que se muestra a través de un *.json* el `echo "Hello"` como le habíamos dicho que hiciera.
 
Si bien es cierto que aquí es bastante simple, pasad a imaginaros lo mismo pero con 50 tareas, todas totalmente distintas...

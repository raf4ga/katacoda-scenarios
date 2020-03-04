Para este curso vamos a utilizar una máquina **CentOS 7**, distribución que suele utilizarse para crear entornos de producción y que cuenta con mucha documentación sobre cómo solucionar todo tipo de problemas. 

Como tenemos una **máquina limpia**, sin mucho más que lo estrictamente necesario para funcionar, **vamos a tener que instalarnos nosotros mismos Ansible**. El objetivo de hacerlo en esta etapa es darnos cuenta de que Ansible no es única y exclusivamente eso, sino que se sirve de una serie de dependencias que veremos a continuación.

Para **instalar Ansible**, tendríamos que utilizar nuestro gestor de paquetes con el siguiente comando ``apt install ansible``. en este caso, ya está incluido en el entorno.

# ¿Qué instalamos junto con Ansible?

Si ejecutemos el comando, veremos cómo resuelve toda la serie de dependencias del paquete, y antes de que escribamos "y" y pulsemos Enter para instalarlo, podremos fijarnos en que **Ansible no viene solo**.

En efecto, **Ansible se sirve de Python para funcionar**. Junto a ello necesitará, además, ser capaz de establecer conexiones SSH. Además de la estructura de ficheros, escritos en YAML, Ansible se sirve de Python para crear sus distintos módulos de automatización. 

Desde módulos para control de ficheros hasta módulos de instalación de paquetes: todo (más bien CASI todo) funcionando a través de python, **sin ejecutarse nativamente** en las máquinas donde lo llamamos.

¿Quiere decir, entonces, que en todas las máquinas que queramos automatizar con Ansible vamos a necesitar instalar todo esto?

Lo bueno es que la respuesta aquí es **NO**.

# El maestro de Ansible y sus Esclavos

Ansible funciona con una estructura en la que **una máquina hace de maestro o nodo controlador**, mientras que todas las demás, generalmente, actúan como esclavas de este maestro.

Lo mejor de todo, es que Ansible **SOLO** tiene que instalarse en la máquina que hace de controlador.

**Para hacer funcionar los esclavos simplemente necesitaremos que esté instalado Python en ellos**, ya que como habíamos explicado antes, es la parte que se encarga de la ejecución.

Con esto listo, **el maestro se encargará de establecer las conexiones y delegar las distintas tareas a sus nodos** para que, a través de un comando muy simple, logremos preparar una serie de máquinas en muy poco tiempo, sin estar conectándonos nosotros directamente y ejecutando código nativamente.

Con esto listo y entendido, ya podemos comenzar con la creación y utilización de Ansible en nuestro pequeño entorno de pruebas.

**A partir de aquí, ¡ya todo queda a tu discreción!**

Aún así, entiendo que todo se hace más llevadero si alguien te propone una idea a desarrollar con lo que sea que estés aprendiendo a utilizar, así que... Aquí van unas cuantas ideas para practicar:

# Crea un script, y ejecútalo.
El directorio 'files' puede utilizarse no solo para guardar ficheros para copiar, sino que también puede albergar scripts a ejecutar por Ansible. 

El mismo cuenta con un módulo con documentación a la cual puedes acceder utilizando este [enlace](http://docs.ansible.com/ansible/latest/script_module.html). 

Intenta crear un script en bash (.sh) que escriba algo por pantalla, o haga un echo de algo contra un fichero, o que calcule un número y lo guarde en un fichero... ¡haz volar tu imaginación!


# Crea usuarios de manera recursiva
Ansible es capaz de hacer que una tarea se ejecute de manera recursiva. Existe una opción llamada "with_items" que te permite esto, y a esta puedes darle no solo distintas variables (como por ejemplo, 5 nombres de usuario a utilizar por una tarea que crea dichos usuarios) si no que podrías incluso guardar estas en una carpeta llamada "defaults" bajo el directorio del rol y dentro de su main.yml escribirlo ahí como si fuera un vector.

Los loops están documentados [aquí](http://docs.ansible.com/ansible/latest/playbooks_loops.html).

Y la documentación sobre las variables [aquí](http://docs.ansible.com/ansible/latest/playbooks_variables.html).

Investigar por uno mismo es la mejor manera de aprender y retener los conocimientos... ¡así que no tengas miedo!

Si quieres un entorno limpio para trabajar en esto... puedes probar nuestro [Ansible Playground](https://katacoda.com/devopstf/scenarios/ansible-playground).

# Más allá de KataCoda
Podrías crearte tu entorno de pruebas de Ansible utilizando Docker. Para Windows 7 cuentas con Docker Toolbox, que te facilita su instalación, pero en Windows 10 cuentas con una CLI pura con la que controlarlo todo más al estilo 'tío de sistemas'. 

Podrías crearte un contenedor con un CentOS 7, instalarle Ansible y jugar: crearte un servidor apache a través de Ansible, conectar varios contenedores para jugar con las relaciones maestro esclavo, crear tu propio rol de inicialización de máquinas/entornos... 

*You can do it!*

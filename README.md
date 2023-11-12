## Descripcion del playbook

* Este playbook fue pensado para obtener reportes de un servidor remoto, y enviar esos reportes al correo del superior.

### Requerimiento antes de usar el playbook

* Configurar nuestro correo de Gmail para el uso de una aplicacion de tercero que este caso es Ansible. Para mas informacion de como se configura en gmail visite el siguiente enlace: [Link](https://www.redhat.com/sysadmin/configure-gmail-using-ansible)

### Modo de Uso del Playbook

* Tener creado nuestro inventario con los servidores o servidor donde se van a extraer los reportes. En este proyecto deje a modo de ejemplo un archivo llamado inventario.ini como referencia.

* Tener presente que los reportes recolectados se van a almacenar en el directorio **files**, ahi es donde reposaran estos reportes, y por cada vez que se ejecute el playbook todo el contenido de esa directorio sera eliminado para que no se cruce con los reportes nuevos.

* En el directorio handlers se encuentra el archivo main.yml, en el es donde se invoca al modulo de mail que es el responsable de que se ejecute el envio del correo, con los parametros seteados de acuerdo a su informacion. En el campo **body** le puede poner el mensaje que quiere remitir en el correo, para este caso se puso lo siguiente:

```vim
    body: |
      {{ ansible_managed | comment }}  # Variable de presentacion declarada en defaults/main.yml

      Buenas noches ings, 

      Envio los siguientes reportes:

      {% for reportes in reporte %}  # Este bucle es para dar un print en el cuerpo del correo
        - {{ reportes }}             # sobre los archivos que se estaran adjuntando.
      {% endfor %}

      Atentamente,

      Ansible
```

* Los datos sensibles como el usuario y password (No el de la cuenta de gmail, si no el que genera para el uso de las apps de terceros) en este caso del proyecto lo proyecte para que se use como archivo cifrado en la carpeta vars y el archivo secrets.yml. Se tiene pensado asi para proteger estos datos, y asi mismo se invoquen como extra-vars, ya que como se quiere dejar este proceso de forma programada para que todos los dias se ejecute, en el job quedaria de la siguiente forma:

```vim
ansible-playbook -i inventario.ini envia_reportes.yml -e @vars/secrets.yml --vault-password-file /ruta/vault/passwd-vault.prv
```

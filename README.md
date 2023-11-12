### Descripcion del playbook

- Este playbook fue pensado para obtener reportes de un servidor remoto, y enviar esos reportes al correo del superior.

### Requerimiento antes de usar el playbook

- Configurar nuestro correo de Gmail para el uso de una aplicacion de tercero que este caso es Ansible. Para mas informacion de como se configura en gmail visite el siguiente enlace: [Link](https://www.redhat.com/sysadmin/configure-gmail-using-ansible)

### Modo de Uso del Playbook

- 1. Tener creado nuestro inventario con los servidores o servidor donde se van a extraer los reportes. En este proyecto deje a modo de ejemplo un archivo llamado inventario.ini como referencia.

- 2. Tener presente que los reportes recolectados se van a almacenar en el directorio **files**, ahi es donde reposaran estos reportes, y por cada vez que se ejecute el playbook todo el contenido de esa directorio sera eliminado para que no se cruce con los reportes nuevos.

- 3. Los datos sensibles como el usuario y password (No el de la cuenta de gmail, si no el que genera para el uso de las apps de terceros) en este caso del proyecto lo proyecte para que se use como archivo cifrado en la carpeta vars y el archivo secrets.yml. Se tiene pensado asi para proteger estos datos, y asi mismo se invoquen como extra-vars, ya que como se quiere dejar este proceso de forma programada para que todos los dias se ejecute, en el job quedaria de la siguiente forma:

```
ansible-playbook -i inventario.ini envia_reportes.yml -e @vars/secrets.yml --vault-password-file /ruta/vault/passwd-vault.prv
```

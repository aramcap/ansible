# Comandos Ad-Hoc

Siempre contará con el siguiente formato:

```bash
ansible -i <ARCHIVO_INVENTARIO> <FILTRO_INVENTARIO> <ACCIONES>

ansible -i inventario <FILTROS: all, vm01, vm02, cluster> <ACCIONES: -m para emplear módulo, -a para pasar argumentos o comandos>
```

## Ejemplos

```bash
# Try to connect to host, verify a usable python and return pong on success
ansible -i inventario all -m ping

# Check if hosts has ping with remote destination
ansible -i inventario all -a "ping -c 4 8.8.8.8"

# Gathers facts about remote hosts
ansible -i inventario all -m setup
```

Podemos añadir paralelismo con el argumento `-f` (forks):
```bash
ansible -i inventario all -a "ping -c 4 8.8.8.8" -f 2
```

Podemos llevar a cabo acciones en nombre de otro usuario con el argumento `-u` (user):
```bash
ansible -i inventario all -m authorized_key -a "user=user2 key='ssh-rsa AAAA...XXX == root@hostname'" -u user2
```

Podemos llevar a cabo acciones de administrador (sudo) con el argumento `--become`:
```bash
ansible -i inventario all -m authorized_key -a "user=root key='ssh-rsa AAAA...XXX == root@hostname'" --become
```

```bash
# Basic commands
ansible -i inventario all -a "cat /etc/system-release"
ansible -i inventario all -a "systemctl status rsyslog"

# Copy authorized_key
ansible -i inventario all -m authorized_key -a "user=root key='ssh-rsa AAAA...XXX == root@hostname'"

# More complex
ansible -i inventario vm02 -m user -a "name=fannn password={{ 'fannn' | password_hash('sha512', 'mysecretsalt') }}" --become
ansible -i inventario vm02 -m file -a "path=/home/vagrant/dir1 state=directory owner=fannn group=fannn"
ansible -i inventario vm02 -m copy -a "src=ad-hoc/ejemplo dest=/home/vagrant/dir1 mode=0777 owner=fannn group=fannn"
ansible -i inventario vm02 -m file -a "path=/home/vagrant/dir1 state=absent" --become
ansible -i inventario vm02 -m user -a "name=fannn state=absent remove=yes" --become

# Packages and services
ansible -i inventario vm01 -m package -a "name=httpd" --become
ansible -i inventario vm01 -m service -a "name=httpd state=started" --become
ansible -i inventario all -a "systemctl status httpd"

# Reboot and Shutdown
ansible -i inventario vm01 -m reboot --become # Este espera a que la vm este levantada de nuevo para concluir el comando
ansible -i inventario vm02 -a "/sbin/shutdown" --become
```

### CONSEJO

No se recomienda emplear comandos ad-hoc en entornos críticos ya que perderemos la posibilidad de tener por escrito todas las acciones llevadas a cabo, por lo que únicamente los debemos usar para realizar acciones de solo lectura.

## Más ejemplos
```bash
# For check pb1
ansible -i inventario all -a "cat /etc/passwd"

ansible -i inventario all -a "ls -l /home/fannn/dir1" --become

ansible -i inventario all -a "cat /home/fannn/dir1/ejemplo" # Aqui obtendremos un error por falta de permisos

ansible -i inventario all -m file -a "path=/home/fannn mode=0777" --become
ansible -i inventario all -m file -a "path=/home/fannn/dir1 mode=0777" --become
ansible -i inventario all -m file -a "path=/home/fannn/dir1/ejemplo mode=0777" --become

ansible -i inventario all -a "cat /home/fannn/dir1/ejemplo"

# For check pb2
ansible -i inventario all -a "cat /etc/passwd"

ansible -i inventario all -a "ls -l /home/fannn"

# For check pb3
ansible -i inventario vm01 -m setup | grep JAVA_HOME

# For check pb4
ansible -i inventario all -a "systemctl status docker"
```
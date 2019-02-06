# Ejercicio1 Ansible

Adrian Ramos - 2 de Febrero de 2018

---

Vamos a desplegar un contenedor de Docker con Ansible. El contenedor será un servidor Apache (httpd) con un index.html personalizado.

## Requerimientos
* python >= 2.6
* Ansible: http://docs.ansible.com/ansible/latest/intro_installation.html
* Docker: https://docs.docker.com/docker-for-mac/install/
* docker-py >= 1.7.0 : ```sudo pip install docker-py```

## Ejercicio

Vamos a crear una estructura de carpetas para Ansible tal como hemos visto en la formación.

**No será necesario usar inventario pues trabajaremos con nuestro equipo en local, por lo que únicamente tendremos que escribir el archivo del playbook**.

Al ejecutar debemos obtener una salida similar a esta (obviando las alertas que se muestran, pues son relativas al inventario):
```bash
$ ansible-playbook /Users/aramos/ansible/playbooks/docker_apache_create.yml
 [WARNING]: Unable to parse /etc/ansible/hosts as an inventory source

 [WARNING]: No inventory was parsed, only implicit localhost is available

 [WARNING]: Could not match supplied host pattern, ignoring: all

 [WARNING]: provided hosts list is empty, only localhost is available


PLAY [localhost] ************************************

TASK [Gathering Facts] ******************************
ok: [localhost]

TASK [TAREA1] *****************************************************
changed: [localhost]

PLAY RECAP ******************************************
localhost                  : ok=2    changed=1    unreachable=0    failed=0
```

Para aquellos que no estéis familiarizados con *Docker*, indicaros que únicamente tendremos que usar las opciones:
* image: httpd:alpine
* ports: "80:80"

Os dejo una estructura base de la que podéis partir:

```yaml
---
- hosts: localhost
  connection: local

  tasks:
  - name: TAREA 1
    ...
```

## Recomendaciones

Os recomiendo que uséis Visual Studio Code con la extensión de [Ansible](https://marketplace.visualstudio.com/items?itemName=vscoss.vscode-ansible). Incluye sintaxis resaltada y snippets de código.

A partir de aquí, el resto es vuestro! Ánimo!
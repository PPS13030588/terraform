# 3.2 Configurar una máquina virtual Ubuntu 24.04 en Virtualbox mediante Ansible.

- Realizar update & upgrade del sistema de forma automática. 
- Instalar el servicio apache. 

Para esta tarea, he instalado ansible en una máquina virtual Ubuntu Desktop 24.04.1

![inicio_ansible](https://github.com/PPS13030588/terraform/blob/main/images/instalar_ansible.png)

Después de instalar y verificar la instalación de ansible, debemos configurar SSH debido a que es este protocolo el que usa para comunicarse con las máquinas.

```ssh-keygen -t rsa -b 4096 -C "asirfmr@gmail.com"```

![inicio_ansible](https://github.com/PPS13030588/terraform/blob/main/images/generar_ssh_ansible.png)


Y se procede a copiar esta id al host que configurará ansible.

``` ssh-copy-id administrador@192.168.1.159  ```

![inicio_ansible](https://github.com/PPS13030588/terraform/blob/main/images/ssh_añadida_ansible.png)

Es el momento de crear un directorio de configuración para ansible

``` sudo mkdir -p /etc/ansible ```

Editamos el fichero **/etc/ansible/hosts** que gestionará los hosts controlados por ansible, añadiendo la siguiente información:

```yaml 
[servidores]
192.168.1.159 ansible_user=vagrant ansible_ssh_private_key_file=./ssh/id_rsa
```
Y por último, se edita el fichero **/etc/ansible/setup_ubuntu.yml** que será el **playbook** de nuestra tarea, indicando las acciones que debe completar en nuestra máquina remota, es decir, actualizar la máuqina e instalar apache.

```yaml
--
- name: PPS - FMR -  Actualizar sistema e instalar Apache
  hosts: all
  become: yes

  tasks:
    - name: Actualizar la lista de paquetes
      apt:
        update_cache: yes

    - name: Actualizar paquetes instalados
      apt:
        upgrade: dist
        autoremove: yes
        autoclean: yes

    - name: Instalar Apache
      apt:
        name: apache2
        state: present

    - name: Asegurarse que el servicio Apache está iniciado y habilitado
      service:
        name: apache2
        state: started
        enabled: yes
        
```
Una vez configurado el entorno, ya se puede lanzar el comando que desplegará las acciones con el siguiente comando: 

``` ansible-playbook -i hosts setup_ubuntu.yml --ask-become-pass ```


![inicio_ansible](https://github.com/PPS13030588/terraform/blob/main/images/ansible_ejecutado_ok.png)

Si accedemos a la IP de la máquina configurada, obtendremos la página de bienvenida de Apache.

![inicio_ansible](https://github.com/PPS13030588/terraform/blob/main/images/apache_ansible.png)


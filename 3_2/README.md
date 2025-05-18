# Configurar una máquina virtual Ubuntu 24.04 en Virtualbox mediante Ansible.

- Realizar update & upgrade del sistema de forma automática. 
- Instalar el servicio apache. 

Para esta tarea, he instalado ansible en una máquina virtual Ubuntu Desktop 24.04.1

![inicio_ansible](https://github.com/PPS13030588/terraform/blob/main/images/instalar_ansible.png)

Después de instalar y verificar la instalación de ansible, debemos configurar SSH debido a que es este protocolo el que usa para comunicarse con las máquinas.

``` ssh-keygen -t rsa -b 4096 -C "asirfmr@gmail.com" ´´´

![inicio_ansible](https://github.com/PPS13030588/terraform/blob/main/images/generar_ssh_ansible.png)


Y se procede a copiar esta id al host que configurará ansible.

``` ssh-copy-id administrador@192.168.1.159 ´´´

![inicio_ansible](https://github.com/PPS13030588/terraform/blob/main/images/ssh_añadida_ansible.png)


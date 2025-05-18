# 3.3  Configurar una máquina virtual Ubuntu 24.04 en Virtualbox mediante Ansible. 

- Crear un index.html con el contenido: ‘Ansible rocks’ en el directorio del servidor web para 
poder ser mostrado y reiniciar el servicio. 

- Realizar un curl al servidor web y verificar el mensaje ‘Ansible rocks’. 

Este apartado se podría considerar una ampliación muy interesante del anterior 3.2. 

En este caso, se prepara un playbook.yml que creará un **index.html** que contiene el mensaje especificado "Ansible rocks".

El fichero **setup_3_3.yml** es el siguiente:

```yml 
---
- name: Configurar Ubuntu 24.04 con Apache y página personalizada
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

    - name: Crear archivo index.html con contenido personalizado
      copy:
        dest: /var/www/html/index.html
        content: "Ansible rocks\n"
        owner: www-data
        group: www-data
        mode: '0644'

    - name: Reiniciar servicio Apache
      service:
        name: apache2
        state: restarted

    - name: Verificar página web con curl
      command: curl -s http://localhost
      register: curl_result

    - name: Mostrar resultado de curl
      debug:
        msg: "Contenido de la página web: {{ curl_result.stdout }}"

    - name: Validar que el contenido es 'Ansible rocks'
      assert:
        that:
          - "'Ansible rocks' in curl_result.stdout"
        fail_msg: "El contenido no coincide."
        success_msg: "El contenido coincide correctamente."
```

![inicio_ansible](https://github.com/PPS13030588/terraform/blob/main/images/ansibleRock.png)

En la imagen anterior se puede ver cómo se ha ejecutado el guión, tal y como lo hemos especificado en el fichero .yml.

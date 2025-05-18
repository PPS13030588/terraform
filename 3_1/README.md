## 3.1 Provisionar una máquina virtual Ubuntu 24.04 en VirtualBox mediante Terraform

En esta sección se detalla el proceso para provisionar una máquina virtual con Ubuntu 24.04 utilizando Terraform y VirtualBox como proveedor de virtualización.

Terraform automatiza la creación y configuración inicial de la máquina virtual, incluyendo:

- Creación de la VM con nombre y sistema operativo especificado.
- Asignación de recursos como CPU, memoria RAM y disco duro.
- Configuración de red (por ejemplo, NAT o puente).
- Registro e inicio automático de la máquina virtual.

Este enfoque permite crear entornos reproducibles y consistentes, facilitando la integración con herramientas de automatización posteriores, como Ansible, para la configuración del sistema operativo y despliegue de aplicaciones.

A continuación, se incluirá un ejemplo básico del archivo `main.tf` para realizar esta provisión.

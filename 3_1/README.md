## 3.1 Provisionar una máquina virtual Ubuntu 24.04 en VirtualBox mediante Terraform

En esta sección se detalla el proceso para provisionar una máquina virtual con Ubuntu 24.04 utilizando Terraform y VirtualBox como proveedor de virtualización.

Terraform automatiza la creación y configuración inicial de la máquina virtual, incluyendo:

- Creación de la VM con nombre y sistema operativo especificado.
- Asignación de recursos como CPU, memoria RAM y disco duro.
- Configuración de red (por ejemplo, NAT o puente).
- Registro e inicio automático de la máquina virtual.

Este enfoque permite crear entornos reproducibles y consistentes, facilitando la integración con herramientas de automatización posteriores, como Ansible, para la configuración del sistema operativo y despliegue de aplicaciones.

A continuación, se incluirá un ejemplo básico del archivo `main.tf` para realizar esta provisión.

### main.tf
```
terraform {
  required_providers {
    virtualbox = {
      source  = "shekeriev/virtualbox"
      version = "0.0.4"
    }
  }
}

provider "virtualbox" {}

resource "virtualbox_vm" "ubuntu_2404" {
  name   = "PPS_RA5_2_ubuntu-24.04"
  image  = "https://vagrantcloud.com/generic/ubuntu2404/versions/2.1.26/providers/virtualbox.box"
  cpus   = 2
  memory = "2048 mib"

  network_adapter {
    type   = "nat"
    device = "IntelPro1000MTDesktop"
  }
}

```
Se ha creado un directorio específico para Terraform donde voy añadiendo los ficheros necesarios para el correcto funcionamiento del despliegue de las máquinas.

Una vez añadido el fichero **main.tf**, desde la consola de comandos de Windows se ejecuta ``` terraform init ``` y una vez finalizado el proceso, se puede lanzar el ``` terraform plan ``` que nos mostrará cuál va a ser el proceso de despliegue de la máquina virtual.

Verificado el proceso que orquesta el **maint.tf**, se procede a lanzar el despliegue planeado mediante el comando ``` terraform apply ´´´ 


Finalmente podemos comprobar que la máquina virtual se ha añadido, configurado e iniciado en VirtualBox, conforme a los requisitos añadidos en el ###main.tf.

![inicio_terraform](https://github.com/PPS13030588/terraform/blob/main/images/virtualbox1.png)
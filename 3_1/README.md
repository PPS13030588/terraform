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
``` resource "null_resource" "ubuntu_vm1" {
  triggers = {
    always_run = timestamp()
  }

  provisioner "local-exec" {
    command = <<EOT
      VBoxManage createvm --name PPS_RA5_2_ubuntu2404 --ostype Ubuntu_64 --register
      VBoxManage modifyvm PPS_RA5_2_ubuntu2404 --memory 2048 --cpus 2 --nic1 nat
      VBoxManage createhd --filename "C:\\terraform\\PPS_Terraform_VirtualDisk.vdi" --size 20000
      VBoxManage storagectl PPS_RA5_2_ubuntu2404 --name "SATA Controller" --add sata --controller IntelAhci
      VBoxManage storageattach PPS_RA5_2_ubuntu2404 --storagectl "SATA Controller" --port 0 --device 0 --type hdd --medium "C:\\terraform\\PPS_Terraform_VirtualDisk.vdi"
      VBoxManage storageattach PPS_RA5_2_ubuntu2404 --storagectl "SATA Controller" --port 1 --device 0 --type dvddrive --medium "C:\\terraform\\ubuntu-24.04.2-live-server.iso"
      VBoxManage startvm PPS_RA5_2_ubuntu2404 --type headless
    EOT
  }
}
```

# Libreboot un x60

[🏠Inicio](../../README.md)

## Índice:
* []()

------

## Seguín este video de Luke Smith
[Luke Smith - Install Libreboot on X60/T60 ThinkPads (muh freedumbs edition)](https://www.youtube.com/watch?v=1K5jo9gk9LQ)

## Erros que me fun atopando
Ao facer o primeiro flash saiame un erro sobre /dev/mem que me impedía o proceso. Mirando nesta [páxina](https://www.flashrom.org/FAQ#What_can_I_do_about_.2Fdev.2Fmem_errors.3F) puiden ver como ese error típico se solventaba poñendo "iomem=relaxed" ao comando de inicialización do grub.  
  
Editando o ficheiro da seguinte forma "sudo vim /etc/default/grub", na liña "GRUB_CMDLINE_LINUX_DEFAULT" engadinlle o seguinte entre as comillas desa liña " iomem=relaxed" seguindo o exemplo deste [post](https://askubuntu.com/questions/19486/how-do-i-add-a-kernel-boot-parameter).  
  
Reiniciei e ao facer o flash todo correcto.

## Paquete powertop
Ao estar utilizando void en lugar de parabola ou arch o proceso de instalación foi co comando "sudo xbps-install -S powertop".  
  
Para facer que se iniciase cando se inicia o so o que fixen foi crear un ficheiro tal que "sudo vim /etc/runit/core-services/powertop_eu.sh" cos contidos:  
	msg "Powertop autotune"  
	powertop --auto-tune  
  
Logo fíxeno executable co comando "sudo chmod +x /etc/runit/core-services/powertop_eu.sh".  
  
Seguín este [tutorial](https://caioalonso.com/2018/11/05/running-powertop-on-boot-on-void-linux.html) para facer que se executase ao iniciar.

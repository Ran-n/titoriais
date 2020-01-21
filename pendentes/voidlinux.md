% Instalación de Void Linux
% baixar o iso na páxina, eu collín o live-i3686-2018 (o sen ningún escritorio de 32 bits sacado no 2018)

% PASO 1
% coller un usb e facelo bootelabe
sudo dd if=imaxe.iso of=/dev/sdb status="progress"

% PASO 2
% bootear dende o usb, configurar a BIOS se é preciso
% entrar na primeira opción e esperar que nos poña o login e contrasinal
% como temos a versión sen escritorio deberemos entrar por terminal
login: root
password: voidlinux

% poñemos bash para poder subir nos antigos comandos etc
bash
% cambiamos o layout do noso teclado a español
loadkeys es

% como nas outras formas de void linux iniciamos o instalador
void-installer

% unha vez executado sairanos unha pantalla que nos guiará polo proceso
% keyboard
% movemonos coas teclas de dirección até atopar es ou pulsamos "e" repetidas veces até atopalo

% network
usarase ethernet soamente, na wiki explica por se queres facer a instalación usando wifi

% source

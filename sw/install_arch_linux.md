# Instalación de Arch Linux

## 1º parte, crear o usb booteable

### Comando para mostrar os discos no sistema.
	
	$ lsblk

### Montamos a imaxe que antes tivemos que descargar da páxina oficial ().
É importante asegurarse de que o ficheiro 'of' corresponde ao usb no que se fará a instalación, senón podese borrar todo o sistema no que se está a executaro o comando.
A opción 'status progress' é para que mostre como vai senón non sae nada tras executar o comando.
 
	# dd if=folder/arch.iso of=/dev/sda status="progress"

## 2º parte, iniciar co usb pinchado

### Meter o pincho no ordenador no que queremos instalalo.
Temos que ter cambiado na bios para que bootee dun usb como primeira opción.
Iniciar (ou gardar e sair se fixeches o paso anterior de cambiar as opcións da bios) o ordenador co usb pinchado (importante: posto para que bootee de pincho, paso anterior).

### Durante o arranque deberase presionar a tecla especial para que te conduza á bios (pode ser F2 ou F12 ou Esc).

### Cambiar a configuración do teclado a español (de se ese a túa distribución de teclado):

	loadkeys es

### Mirar os discos
	
	lsblk

### Mirar que tipo de BIOS estas usando

	ls /sys/firmware/efi/efivars
 
Debería sair que non existe o ficheiro ou directorio, do contrario tes outro tipo de BIOS ao que usaremos neste tutorial.
Isto cambiará como particionas os drives e como instalas o bootloader, busca outro tutorial se acaso.

### Comprobar que temos internet, dúas opcións:

#### 1 > Conectar o cable ethernet ao router

	ping duckduckgo.es

Deben sair os paquetes mandandose, 'ctr+c' para parar a pantalla de envío.

#### 2 > Peor porque pode dar problemas ao configurar e é mais lento:

	wifi-menu

O comando, buscará todas as redes wifi e mostrarache un menú con elas.
Elixes a túa, daslle ok, metes o contrasinal, daslle ok e terás internet.
A forma de probar a conexión é a mesma ca na opción 1.

Mellor usar a primeira opción se se pode, máis rápido e sen necesidade de configurar nada.

### Agora vamos configurar o reloxo:
	
	timedatectl set-ntp true

### Agora particionaremos o drive:
Facendo lsblk veremos todos os drives e as súas particións, deberemos ver o nome do que queremos instalar (normalmente '/dev/sda').
Unha boa forma de saber que disco e que é mirando o tamaño que ocupa.

Por que particionar? É mellor a aproximación modular por se hai algún problema no sistema.

	fdisk /dev/sda

Agora estamos dentro da liña de comandos de fdisk, poñemos p para imprimir a info do disco.

#### Se tes algunha partición no disco deberás eliminala
Usando d, se tes varias diráche que elixas o número de partición (pos d e intro).
Sempre e cando queiras ter só instalado arch no dispositivo, se queres varios OS pois deberás ter as particións adecuadas para cada un.

	d

#### Unha vez non hai particións creamos as novas

##### Esta primeira será a nosa partición boot

	n

Agora preguntaranos unha serie de cousas sobre a partición.

###### Tipo da partición: primaria
	
	p

###### Número da partición
Usamos a por defecto.

###### Primeiro sector
Usamos o por defecto.
Sse o estivesemos instalando con outro sistema operativo teríamos que ter coidado con esto porque poderíamos sobrescribir outra partición.


###### Último sector:

	+200M

Se antes tiñas unha partición pedirache que se queres eliminar a sinatura, debes poñer y para yes

Usando p veremos como a nova partición está creada correctamente

##### Esta será a nosa partición swap

	n

###### type
Primaria

	p

###### Number
Por defecto

###### primeiro sector
Por defecto

###### Último sector
Unha boa regra é usar 150 da túa memoria RAM (se tes 8GB RAM -> 12GB swap).
Usaremos o exemplo de que tes 4GB de RAM.

	+6G

Se antes tiñas unha partición pedirache que se queres eliminar a sinatura, debes poñer y para yes.

Usando p veremos como a nova partición está creada correctamente

##### Esta será a nosa partición root ( / )
Onde estarán todos os nosos programas.

	n

###### type
Primaria

	p

###### Number
Por defecto

###### Primeiro sector
Por defecto

###### Último sector
Usar un rango de 15G-25G dependendo dos teus hábitos de instalación de programas etc (se tes espazo suficiente usar 25G).

	+25G

Se antes tiñas unha partición pedirache que se queres eliminar a sinatura, debes poñer y para yes

Usando p veremos como a nova partición está creada correctamente

##### Esta será a nosa partición /home
Onde gardamos os ficheiros e demais cousas do usuario.

	n

##### type
Primaria

	p

###### Number
Por defecto

###### Primeiro sector
Por defecto

###### Último sector
Aquí depende de se só queres instalar arch ou queres outro OS.
Se queres outro en dual, triple, etc boot pois colles x GB para o home (se xeneroso).
Deixándoo vacío de dándolle intro collerá todo o espazo sobrante.

Usaremos o exemplo de só querer instalar arch

Se antes tiñas unha partición pedirache que se queres eliminar a sinatura, debes poñer y para yes

Usando p veremos como a nova partición está creada correctamente

Agora para gardar os cambios usaremos o comando w.
Cerciónate de que todo está como queres porque se borrará todo o que había nese disco.

	w


###  Agora usando lblk veremos as novas particións
lsblk

Asumindo que todo está correcto.

### Sistemas de ficheiros nas particións de discos necesarias.

As que queremos facer sistemas de ficheiros son a partición boot(1), root(3), home(4)
A swap(2) non terá ficheiros dentro así que non é necesario facerlle nada neste paso.

#### Para a boot
	mkfs.ext4 /dev/sda1

#### Para a root(3)
	mkfs.ext4 /dev/sda3

#### Para a home(4)
	mkfs.ext4 /dev/sda4

### Que a partición swap(2) actúe como swap.
Facemos que sexa unha partición swap.
	
	mkswap /dev/sda2

Facemos que faga swap nesa partición,

	swapon /dev/sda2

Facendo lsblk de novo veremos como para a partición do swap aparece o texto [SWAP]

	lsblk

### Montar as particións para poder modificalas.

#### Primeiro a partición root

	mount /dev/sda3 /mnt

Facendo lsblk veremos que aparece a info na columna MOUNTPOINT
 
#### Agora creamos os directorios home e boot para montar as particións neles.

	mkdir /mnt/boot
	mkdir /mnt/home

Para a partición boot

	mount /dev/sda1 /mnt/boot

Para a partición home

	mount /dev/sda4 /mnt/home

Se facemos lsblk veremos como todo está montado no /mnt correspondente
	
	lsblk

### Agora instalaremos Archlinux:

Instalao usando /mnt como base, e damoslle paquetes básicos para que instale e programas:
* wireless tools para ter iwconfig.
* dialog para ter wifi-menu na instalación.

O comando.

	pacstrap /mnt base base-devel vim

### Ficheiro fstab
Serve para automontar as particións.
Usando -U usa UUID en lugar de nomes

	genfstab -U /mnt >> /mnt/etc/fstab

### Bootloader
Instalamos o bootloader para poder cargar o sistema ao iniciar.
Metémonos no sistema que estamos configurando.

	arch-chroot /mnt

### NetworkManager
Instalamos un network manager e activamolo para ter internet ao iniciar a instalación fresca

	pacman -S networkmanager

Dicímoslle a systemd que o auto inicie ao iniciar o ordenador, vale calquer dos dous.

	systemctl enable NetworkManager

ou

	systemctl enable NetworkManager.service

### Grub

#### Instalación do Grub

Instalamos grub, sirve para ler e bootear na partición durante o arranque.

	pacman -S grub

Instalará grub no drive seleccionado.

	grub-install --target=i386-pc /dev/sda

Se da problemas.

	grub-install --force --target=i386-pc /dev/sda

#### Configuración do grub
	
	grub-mkconfig -o /boot/grub/grub.cfg

### Contrasinal do root

	passwd

### Locale
Agora vamos elixir o locale

	vim /etc/locale.gen

Neste ficheiro deberemos descomentar as liñas que nos interesen segundo o idioma que queremos.

Gardaremos o ficheiro e executamos o seguinte para crear o locale deses idiomas

	locale-gen

Poñeremos a nosa variable de idioma

	vim /etc/locale.conf

Exemplo para o inglés:
___
LANG=en_US.UTF-8
___

### Zona de tempo

Exemplo para alguén que viva na zona horaria de Phoenix:

	ln -sf /usr/share/zoneinfo/America/Phoenix /etc/localtime

Exemplo para alguén que viva na zona horaria de Madrid:

	ln -sf /usr/share/zoneinfo/Europe/Madrid /etc/localtime

Isto creará un softlink entre a hora da zona dada e o localtime da máquina.

Se cambiaramos de zona deberíamos reexecutar este comando.

### Hostname

Poñeremos un hostname (nome do ordenador)
	
	vim /etc/hostname

Exemplo dun hostname:
___
archie
___


### Saida do livecd

Agora saimos da nosa configuración dentro do live device

	exit

### Desmontaxe

Desmontamos todo o que montamos anteriormente

	umount -R /mnt

### Apagado
Podemos facer reboot ou poweroff.
Con reboot se somos lentos e quitar o pincho que reiniciará na nosa configuración de arch.

	reboot

ou

	poweroff


### Logueo

Unha vez iniciada a máquina deberíamos poder entrar como:
	
	root

e 

	o contrasinal elixido 

e poderemos facer ping e demais cousas.

## 3º parte, configuración

 para gardar o teclado coa configuración que queremos faremos o seguinte (no exemplo poñemos o dun teclado en español)
echo KEYMAP=es >> /etc/vconsole.conf

 para ver todas as opcións que queremos
localectl list-keymaps

 engadir un usuario
useradd -m usuario
 engadir contrasinal ao usuario
passwd usuario

 facer o usuario sudo
 neste exemplo faremolo sudo e faremos que non nos pida o contrasinal (non é moi seguro así que non é recomendable)
vim /etc/sudoers
________________
usuario ALL=(ALL) NOPASSWD:ALL
________________

 outra opción que tes e poñelo pero sen poñerlle a parte de NOPASSWD: e así pedirache o contrasinal co usuario cada vez (o do usuario)
 outra opción e meter o usuario nun grupo e darlle permisos de root a ese grupo
 etc, busca en opcións

 tocamos o modulo pam de su para ter usuarios que poden facerse su sen pedir contrasinal e usuarios que non poden facerse inda sabendoo
groupadd admins
groupadd normies
 admins poderan su sen saber contrasinal e normies nin sabendoo poderan
 engadimos ao usuario usuario ao grupo de admins
usermod -a -G admins usuario
 facemolo admin do grupo admins e normies
gpasswd -A usuario admins
gpasswd -A usuario normies
 modulos pam
sudo vim /etc/pam.d/su
______________________
# facerse root sen saber o contrasinal
auth sufficient pam_wheel.so trust group=admins

# non se poder facer root inda sabendo o contrasinal
auth requisite pam_wheel.so deny group=normies
______________________


precisas de ethernet ou de telo instalado antes na instalación
 entorno gráfico
sudo pacman -S xorg-server xorg-xinit

 manexador de ventás mostraremos
pacman -S i3-gaps i3status rxvt-unicode dmenu

 fontes de texto para as aplicacións
pacman -S noto-fonts

 o de iniciar sesión, 2 opcións eu uso a segunda
pacman -S lightdm lightdm-gtk-greeter
pacman -S slim

 iniciar o de iniciar sesión
sudo systemctl enable lightdm
sudo systemctl enable slim

 poñer o wifi a funcar
 poñer a configuración de /etc/hosts como se mostra
_________________________
# <ip-address>    <hostname.domain.org>    <hostname>
127.0.0.1      localhost.localdomain    yourHostname
# 127.0.0.1      archie.dev.local         archie
::1            localhost.localdomain    yourHostname
_________________________

 podemos ver o hostname e o hostname.localdomain usando os seguintes comandos
hostname
hostname -f

 podemos ver as interfaces de red
$ lspci | grep -i net

 se non se lista co anterior pode ser que estes nun usb e debes usar o seguinte
$ lsusb 

 comando para ver o estado de tódalas interfaces de rede
$ ip link

 Instala estas cousiñas
$ sudo pacman -S wpa_supplicant wireless_tools networkmanager network-manager-applet gnome-keyring

 Deshabilita dhcpcd porque só pode haber un que se encargue de dhcpcd senón peta
$ sudo systemctl disable dhcpcd.service
$ sudo systemctl disable dhcpcd@.service
$ sudo systemctl stop dhcpcd.service
$ sudo systemctl stop dhcpcd@.service

 Activamos o wpa_supplicant para usar o wifi
$ sudo systemctl enable wpa_supplicant.service

 esto non o fixen
  Add your user to the network group:
 $ gpasswd -a USERNAME network

 Apaga as interfaces
$ ip link set down eth0/enp9s0
$ ip link set down wlan0/wlp8s0

 inicia o wpa_supplicant
$ sudo systemctl start wpa_supplicant.service

 inicia o networkmanager (non debería facer falta, fai un systemctl status NetworkManager se queres saber se precisas ou non inicialo):
$ sudo systemctl start NetworkManager.service

 dicirlle a xorg que iniciar
echo exec i3 >> ~/.xinitrc

 iniciar xorg
startx
 peor
xinit

 dentro de i3 darlle windows+intro e poñer no de comandos o seguinte (ten en conta que estará co teclado en inglés)
$ nm-applet

 sairá un icono de internet e ahí xa te conectas á wifi como sempre

 cambiar o teclado de i3
setxkbmap -layout es
 permanentemente, no ficheiro ~/.config/i3/config engadir ao final
sudo ~/.config/i3/config
_________________________________
exec "setxkbmap -layout es"
_________________________________

 se quixeras ter varios intercambiables
exec "setxkbmap -layout us,de"
exec "setxkbmap -option 'grp:alt_shift_toggle'"

 modificar o grub para o noso estilo
sudo vim /etc/default/grub
_________________
#cambiamos o grub timeout
GRUB_TIMEOUT=1

# cambiamos a imaxe de fondo
GRUB_BACKGROUND=/path/to/file
_________________

 metemos os cambios, en arch non hai un update-grub
sudo grub-mkconfig -o /boot/grub/grub.cfg

 creamos unha carpeta para poñer os nosos scripts
mkdir ~/scripts
 metemola no path
export PATH="$PATH:/path/to/dir"

 para metela permanentemente podemos elixir metela no noso .bashrc ~/.bashrc (se so o queremos para nos) ou en /etc/bash.bashrc
export PATH="$PATH:/path/to/dir"

 configuramos slim
 https://wiki.archlinux.org/index.php/SLiM
 cambiarlle a imaxe de fondo
 hai que ter en conta que primeiro collerá as imaxes que sexan png e logo as jpg
 vamos modificar o default theme
 aseguramonos de que a imaxe de background do default sexa .jgp facendo un ls, se é png
sudo mv /usr/share/slim/themes/default/background.png /usr/share/slim/themes/default/background.jpg

 facemos un soft link da imaxe que queremos á que queremos poñer como fondo en slim
sudo ln -s /path/to/photo /usr/share/slim/themes/default/background.png

 facémolo desta forma porque se pasase algo sempre cargará a anterior imaxe, de non ter imaxe que cargar (tendo eliminado a imaxe por defecto e non tendo feito ben o link da imaxe) quedarase nun bucle infinito e non poderás iniciar sesión.

 instalar para poñer fondo de pantalla, visor de imaxes, para ver videos, para facer screenshots, para facer cousas con imaxes, para mostrar carpetas en árbore
sudo pacman -S xwallpaper sxiv mpv maim imagemagick tree

 configurar o audio
sudo pacman -S pulseaudio pulseaudio-alsa pulsemixer alsa-utils
pulseaudio --start
 rebootear e funcionaba, reboteei sen ter instalado alsa-utils ou ter posto o comando e nada

 tutorial de pacman
-S 		> Sync, instalar ou update paquetes
-Sy 	> sincronizar a base de datos de paquetes
-Su 	> update os paquetes que xa foron detectados con -Sy
-Syu 	> deberías poñelos xuntos
-Syyu 	> para forzar a que remire inda que miraras hai pouco na base de datos de paquetes
-Syuw 	> coa w fai que che baixe os paquetes pero fai que ti os instales manualmente cando queiras
-Ss 	> para buscar paquetes relacionados co paquete dado, colle caracteres de regex (^emacs para que comece con emacs)

 Cando fas un update baixas a nova versión do paquete pero a vella continua no ordenador
-Sc 	> elimina estes paquetes vellos

-R 		> Eliminar
-Rs 	> para eliminar coas dependencias
-Rns 	> para eliminar coas dependencias e os ficheiros de configuración do sistema (non os dot files teus)

-Q 		> lista todos os programas que tes instalados
-Qe 	> programas que instalaches ti persoalmente ou outro programa teu
-Qn 	> os progrmas instalados de repositorio oficial
-Qm 	> os programas instalados de AUR
-Qdt 	> lista dependencias innecesarias (dependencias instaladas cun programa o cal foi desinstalado)
-Qs 	> para buscar un ficheiro no repositorio local

-S/R/Qq > fai que se quite o tipo específico de versión do programa

 para facer pacman e yay coloridos 
grep "^Color" /etc/pacman.conf >/dev/null || sed -i "s/^#Color/Color/" /etc/pacman.conf
grep "^Color" /etc/pacman.conf >/dev/null || sudo sed -i "s/^#Color/Color/" /etc/pacman.conf

 modificamos pacman.conf para ter o ILoveCandy, o primeiro para un usuario o resto para o global
vim /usr/local/etc/pacman.conf 
vim /etc/pacman.conf

grep "ILoveCandy" /etc/pacman.conf >/dev/null || sed -i "/#VerbosePkgLists/a ILoveCandy" /etc/pacman.conf
grep "ILoveCandy" /etc/pacman.conf >/dev/null || sudo sed -i "/#VerbosePkgLists/a ILoveCandy" /etc/pacman.conf

 a opción VerbosePkgLists lista os programas e canto ocupan por individual

 agora mover na lista os servidores para actualizar segundo che queden mais preto
sudo vim /etc/pacman.d/mirrorlist

 configurar para poder instalar paqueter AUR

 instalamos python e pip
sudo pacman -S python python-pip






















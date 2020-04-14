# Alternativas libres ou abertas a programas ou servizos usuais.

[🏠Inicio](../README.md)

## Índice:
* [Cambiar nome de usuario](minitutos.md#Cambiar-nome-de-usuario)
* [Eliminar o ^M ao final de linha](minitutos.md#eliminar-o-^m-ao-final-de-linha)
* [Saber a distribución de Linux](minitutos.md#distribucion-linux)
* [Mostrar espazo en disco](minitutos.md#mostrar-espazo-en-disco)
* [Exemplo pandoc](minitutos.md#exemplo-pandoc)
* [Imprimir json de forma bonita no terminal](minitutos.md#json-terminal)
* [Restaurar usb (Windows)](minitutos.md#restaurar-usb-windows)
* [Redireccionar saída](minitutos.md#redireccionar-saida)
* [Información do monitor](minitutos.md#informacion-do-monitor)
* [Información de vídeo](minitutos.md#informacion-video)
* [Engadir texto ao final de cada liña dun ficheiro](minitutos.md#engadir-texto-ao-final-de-cada-liña-dun-ficheiro)
* [Engadir texto ao comezo de cada liña dun ficheiro](minitutos.md#engadir-texto-ao-comezo-de-cada-liña-dun-ficheiro)
* [Fusionar arquivos liña por liña](minitutos.md#fusionar-dous-liña-por-liña)
* [](minitutos.md#)

------

## Cambiar nome de usuario
	sudo vim /etc/password
	Buscar a columna co teu nome de usuario
	O campo número 5 (cada :) é o do nome de usuario

## Eliminar o ^M ao final de linha
	dos2unix ficheiro
	sed -e "s/\r//g" ficheiro > novoficheiro
	perl -p -e 's/\r//g' ficheiro > novoficheiro

## Distribución Linux
	cat /proc/version
	cat /etc/issue
	cat /etc/os-release

## Mostrar espazo en disco
	df -h
	se queremos só o escritorio home (ou calquer carpeta)
	df -h /home
	con "-T" mostraranos o tipo de sistema de ficheiros
	df -hT /home

## Exemplo pandoc
	Exemplo de markdown a pdf:
	pandoc -f markdown -t pdf referencia.md
	tipos de formato posibles vense na páxina man

## json terminal
	jq . doc.json
	iso mostra todo o contido, de querermos unha subparte do mesmo faríamos
	jq .subparte doc.json
	jq .subparte.subparte doc.json
	
## Restaurar usb windows
	Abrir diskpart (win+r e escribir diskpart)
	List Disk
	Select Disk NumDisco
	clean
	Create Partition Primary
	Active
	Format fs=Fat32 Quick

## Redireccionar saída
	stdout a ficheiro
	> ficheiro
	1> ficheiro

	stderr a ficheiro
	2> ficheiro

	stdout e stderr a ficheiro
	&> ficheiro

## Información do monitor
	xdpyinfo

	para a resolución
	xdpyinfo | grep dimensions
	xdpyinfo | grep -oP 'dimensions:\s+\K\S+'

## Información vídeo
	ffprobe -v quiet -print_format json -show_format -show_streams video.mkv

	para sacar a resolución directamente
	ffprobe -v error -select_streams v:0 -show_entries stream=width,height -of csv=s=x:p=0 video.mp4

## Pacman
	mostrar paquetes sobrantes
	pacman -Qdt

	eliminar paquetes sobrantes
	pacman -R $(pacman -Qdt)

## Engadir texto ao final de cada liña dun ficheiro
	sed 's/$/cadea de texto a meter/' -i ficheiro

## Engadir texto ao comezo de cada liña dun ficheiro
	sed 's/^/cadea de texto a meter/' -i ficheiro

## Fusionar arquivos liña por liña
	pr -tmJ arqv1.txt arqv2.txt arqv3.txt etc > saida.txt

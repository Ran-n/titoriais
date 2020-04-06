# Alternativas libres ou abertas a programas ou servizos usuais.

[🏠Inicio](../README.md)

## Índice:
* [Cambiar nome de usuario](minitutos.md#Cambiar-nome-de-usuario)
* [Eliminar o ^M ao final de linha](minitutos.md#eliminar-o-^m-ao-final-de-linha)
* [Saber a distribución de Linux](minitutos.md#distribucion-linux)
* [Mostrar espazo en disco](minitutos.md#mostrar-espazo-en-disco)
* [Exemplo pandoc](minitutos.md#exemplo-pandoc)
* [Restaurar usb (Windows)](minitutos.md#restaurar-usb-windows)
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

## Restaurar usb windows
	Abrir diskpart (win+r e escribir diskpart)
	List Disk
	Select Disk NumDisco
	clean
	Create Partition Primary
	Active
	Format fs=Fat32 Quick

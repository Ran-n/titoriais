# Alternativas libres ou abertas a programas ou servizos usuais.

[🏠Inicio](../README.md)

## Índice:
* [Cambiar nome de usuario](minitutos.md#Cambiar-nome-de-usuario)
* [Eliminar o ^M ao final de linha](minitutos.md#eliminar-o-^m-ao-final-de-linha)
* [Saber a distribución de Linux](minitutos.md#distribucion-linux)
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

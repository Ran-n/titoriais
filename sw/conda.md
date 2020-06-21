# Comandos para o uso de Conda
Conda é un programa que permite a creación de entornos virtuais para o trabalho con python.

[🏠Inicio](../README.md)

## Índice:
* [Crear un entorno](conda.md#crear-un-entorno)
* [Iniciar un entorno](conda.md#iniciar-un-entorno)
* [Sair dun entorno](conda.md#sair-dun-entorno)

## Iniciar un entorno
conda activate
conda activate NOME

## Sair dun entorno
conda deactivate

## Crear un entorno
conda create --name NOME
conda create -n NOME python=3.6 	# para dicir versión de python
conda create -n NOME scipy=0.15.0	# para dar un paquete en concreto
conda create -n myenv python=3.6 scipy=0.15.0 astroid babel

## Instalar un paquete
Durante a creación do entorno como se mostrou previamente.

Usando o comando:
conda install -n NOME scipy[=0.15.0]

Usando o pip install dentro do entorno
pip install scipy

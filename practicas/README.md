# Prácticas

Aquí encontrarás todas las práticas y ejercicios que se realizarán en todo la unidad. Cabe mencionar que todos los ejercicios se van a realizar en una máquina virtual Ubuntu Server 20.04 LTS con tres tarjetas de red:
* `enp0s3`: tarjeta de red NAT para aceder a internet.
* `enp0s8`: tarjeta de red interna con red `10.33.99.0/24` con IP `10.33.99.4`.
* `enp0s9`: tarjeta de red interna con red `172.16.0.0/16` con IP `172.16.0.4`.

Además se tendrán dos clientes cada uno en una de las redes internas mencionadas arriba:
* Cliente A: con IP `10.33.99.100`
* Cliente B: con IP `172.16.0.100`

## Lista de práticas

* [Práctica 0: Instalación](/practicas/P00-Instalacion/)
* [Práctica 1: Virtual Hosts](/practicas/P01-VirtualHost/)
* [Práctica 2: Mapeo de URLs](/practicas/P02-MapeoURL/)
* [Práctica 3: Negociación de contenidos](/practicas/P03-NegociacionDeContenidos/)
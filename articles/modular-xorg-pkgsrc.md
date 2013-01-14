Title: Instalando modular-xorg desde pkgsrc
Date: 2013-01-14 15:00
Tags: X, Xorg, pkgsrc
Category: NetBSD
Author: Francisco Valladolid
Slug: modular-xorg-pkgsrc
Lang: es

Recientemente tuve la necesidad de configurar *X* en mi *IBM Thinpad T43p*, la cual incluye una tarjeta gráfica *ATI Fire Gl* con 128MB.

Descargué [NetBSD][1] **-current**(4.99.45) desde ftp y proseguí a realizar una instalación completa, no fue hasta que traté de configurar X para poder usar mi *laptop* como *desktop* para hacer tareas comunes, y no conseguí hacer funcionar X correctamente, [XFree86][2] simplemente no trabajó para mí.

Antes de empezar
================
No instalar [XFree86][2] que viene en base, de-seleccionar en el programa de instalación.

Instalando `modular-xorg` desde *pkgsrc*
========================================
Estoy usando [pkgsrc][3] **-current** para aprovechar algunas novedades, las instrucciones para compilar `modular-xorg` desde [pkgsrc][3] son las siguientes:

* Editar `/etc/mk.conf` e incluir:
`X11_TYPE=modular`

* Los paquetes necesarios son los siguientes:

`# cd /usr/pkgsrc/x11/modular-xorg-server`

`# make install clean`

`# cd /usr/pkgsrc/meta-pkgs/modular-xorg-apps`

`# make install clean`

`# cd /usr/pkgsrc/meta-pkgs/modular-xorg-fonts`

`# make install clean`

`# cd /usr/pkgsrc/x11/xf86-input-keyboard`

`# make install clean`

`# cd /usr/pkgsrc/x11/xf86-input-mouse`

`# make install clean`

* Esta es la parte interesante, elegir el controlador de video (*video driver*) que estés usando en tu computadora o *laptop*, en mi caso estoy usando ATI, para ello:

`# cd /usr/pkgsrc/x11/xf86-video-ati`

`# make install clean`

Configurar *Xorg*
=================
Si todo va bien, proseguimos a configurar [Xorg][4] de dos formas.

`# xorgcfg`

El comando anterior inicia el programa de configuración de [Xorg][4] en modo gráfico y el siguiente en modo texto.

`# xorgconfig`

Configurando lo básico en `~/.xinitrc`
======================================
`xclock -geometry 50x50-1-1 &`

`xterm -geometry 80x34-1+1 -bg OldLace &`

`xsetroot -solid Bisque4 &`

`xterm -geometry 80x44+0+0 -bg AntiqueWhite -name login`

`twm`

Iniciando *Xorg*
================
`# xinit`

Referencias
===========
[Capítulo de X en la Guía de NetBSD][5].

[1]: http://www.netbsd.org/ "The NetBSD Project"
[2]: http://www.xfree86.org/ "XFree86&reg; Home to the X Window System"
[3]: http://www.pkgsrc.org/ "pkgsrc"
[4]: http://www.x.org/ "X.Org Foundation"
[5]: http://www.netbsd.org/docs/guide/en/chap-x.html "The NetBSD Guide"


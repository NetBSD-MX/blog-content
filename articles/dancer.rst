Perl Dancer y NetBSD
====================
:date: 2013-06-26 16:40
:author: asarch
:tags: perl, web, lighttpd, dancer, cpan
:category: programacion 
:slug: dancer 
:lang: es

.. contents::

Instalamos el servidor lighttpd:
--------------------------------

    ``pkg_add -v lighttpd ``

Instalamos los módulos de **Perl** con **CPAN** necesarios (algunos de
ellos no están disponibles como paquetes binarios):

::

    cpan install YAML
    cpan install Plack
    cpan install FCGI
    cpan install FCGI::ProcManager
    cpan install Dancer

Configuración
-------------
 * Creamos el script de booteo:

    ``cp /usr/pkg/share/examples/rc.d/lighttpd /etc/rc.d``

 * Lo habilitamos para el inicio:

    ``vi /etc/rc.conf``
    ``lighttpd=YES``

 * Habilitamos el módulo de **FastCGI**:

    | ``vi /usr/pkg/etc/lighttpd/modules.conf``
    | ``server.modules = (``
    |      ``...,``
    |      ``"mod_fastcgi"``   
    | ``)``

 * Configuramos el **lighttpd** para el trabajo con **NetBSD**:

    | ``vi /usr/pkg/etc/lighttpd/lighttpd.conf``
    |     ``var.server_root="/var/www"``
    |     ``#server.event_handler="linux-sisepoll"``
    |     ``#server.network_backend="linux-sendfile"``
    |     ``#server.use_ipv6="enable"``

 * Creamos los directorios necesarios:

   |  ``mkdir /var/www/htdocs``
   |  ``mkdir /var/log/lighttpd``

 * Arreglamos los permisos:

   | ``groupadd lighttpd``
   | ``useradd lighttpd``
   | ``chown -R lighttpd:lighttpd /var/log/lighttpd``

Creando una aplicacion:
-----------------------

 * Creamos una aplicación **Dancer**:

    ``dancer -a MyWeb::App``

 * Iniciamos la aplicación para crear el socket de conexión:

  |  ``export PATH=$PATH:/usr/pkg/lib/perl5/site_perl/bin``
  |  ``cd MyWeb-App``
  |  ``plackup -s FCGI --listen /tmp/socket bin/app.pl``

  Habilitamos el socket en **lighttpd**:

::

         vi /usr/pkg/etc/lighttpd/lighttpd.conf
         $HTTP["url"] =~ "^/app" {
             fastcgi.server += (
                 "/app" => (
                     "" => (
                         "socket" => "/tmp/socket",
                         "check-local" => "disable"
                      )
                 )
             )
         }

Referencias
-----------

-  `perldoc
   Dancer::Deployment <http://search.cpan.org/~xsawyerx/Dancer-1.3091/lib/Dancer/Deployment.pod>`__


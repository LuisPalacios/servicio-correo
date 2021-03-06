# Introducción


Este repositorio contiene un ejemplo sobre cómo poner en marcha un servicio de correo con contenedores Docker (usando fig). Consta de un servidor SMTP con Postfix, un servidor IMAPD con Courier-New y lo que llamo el "Chatarrero" con  Spamassassin, ClamAV y Amavisd-New. 

Encontras un ejemplo de cómo unirlos todos en el fichero fig-template.yml que podrás renombrar a fig.yml, cambia usuarios, contraseñas, nombres de servidores y dominios para adaptarlo a tus necesidades. 
  

              +--------------+            +------------------------------------+
              |   rsyslogd --|----------> |    fluentd.tld.org: fluentd: 24224 |
              |              |            +------------------------------------+
              |  amavisd-new | 
              |    clamav    | 
              | spamassassin |-- 
              |              |  \
              | (chatarrero) |   \
              +--------------+   |
               10024   |         |
                 ^     |         |
                 |     v          \       +-----------------------------------+                   
                 |   10025         +----> | mysqlcorreo.tld.org: mysql: 33000 |
              +--------------+    /       +-----------------------------------+
              |    postfix --|---/
     25,465 --|              |            +------------------------------------+
              |   rsyslogd --|----------> |    fluentd.tld.org: fluentd: 24224 |
              +--------------+            +------------------------------------+
 
 
              +--------------+    +-----> /data/vmail
    143,993 --| courier-imap-|---/
              |              |            +------------------------------------+
              |   rsyslogd --|----------> |    fluentd.tld.org: fluentd: 24224 |
              |              |            +------------------------------------+
              +--------------+
 
              +--------------+ 
            --| imapfilter  -|---> (Conecta con múltiples cuentas vía IMAP)
              |              |            +------------------------------------+
              |   rsyslogd --|----------> |    fluentd.tld.org: fluentd: 24224 |
              |              |            +------------------------------------+
              +--------------+

Consulta este [apunte técnico sobre varios servicios en contenedores Docker](http://www.luispa.com/?p=172) para acceder a otros contenedores Docker y fuentes en GitHub y entender mejor este ejemplo.


# Instalación

Se apoya en [Docker](https://www.docker.com/) y [fig](http://www.fig.sh/index.html). Las imágenes que utilizo son las siguientes:

* GitHub [base-postfix](https://github.com/LuisPalacios/base-postfix)
* GitHub [base-chatarrero](https://github.com/LuisPalacios/base-chatarrero)
* GitHub [base-courierimap](https://github.com/LuisPalacios/base-courierimap)
* GitHub [base-imapfilter](https://github.com/LuisPalacios/base-imapfilter)
* GitHub [base-mysql](https://github.com/LuisPalacios/base-mysql)
* GitHub [base-fluentd](https://github.com/LuisPalacios/base-fluentd)
* GitHub [base-eskibana](https://github.com/LuisPalacios/base-eskibana)

Otro proyecto relacionado que puede interesarte es un contenedor con un cliente WebMail

* GitHub [base-roundcube](https://github.com/LuisPalacios/base-roundcube)


Para instalar y crear los contenedores haz un clone en tu Host

    $  git clone https://github.com/LuisPalacios/servicio-correo.git

Renombra, edita y adapta a tu gusto y necesidades 

    $ mv fig-template.yml fig.yml

Arranca el servicio con el comando siguiente

    $ fig up -d



### Volumen

Directorio persistente para configurar el Timezone. Crear el directorio /Apps/data/tz y dentro de él crear el fichero timezone. Luego montarlo con -v o con fig.yml

    Montar:
       "/Apps/data/tz:/config/tz"  
    Preparar: 
       $ echo "Europe/Madrid" > /config/tz/timezone

# Introducción


Este repositorio contiene un ejemplo sobre cómo montar un servicio de correo electrónico usando Postfix, Courier-New, Spamassassin, ClamAV, Amavisd-New, montando todos los servicios apoyándose en contenedores docker. 

Aquí encontrarás el fichero de ejemplo fig-template.yml. Renombra a fig.yml, cambia usuarios, contraseñas, nombres de servidores y dominios para adaptarlo a tus necesidades. 
  

    +--------------+            +------------------------------------+
    |   rsyslogd --|----------> |    fluentd.tld.org: fluentd: 24224 |
    |              |            +------------------------------------+
    |  amavisd-new | 
    |    clamav    | 
    | spamassassin | 
    |              | 
    | (chatarrero) |
    +--------------+
          10025
            |
            |                   +-----------------------------------+                   
          10024          +----> | mysqlcorreo.tld.org: mysql: 33000 |
    +--------------+    /       +-----------------------------------+
    |    postfix --|---/
    |              |            +------------------------------------+
    |   rsyslogd --|----------> |    fluentd.tld.org: fluentd: 24224 |
    +--------------+            +------------------------------------+
 
 
    +--------------+
    |              |            +------------------------------------+
    |   rsyslogd --|----------> |    fluentd.tld.org: fluentd: 24224 |
    |              |            +------------------------------------+
    |              | 
    |  courier-imap|
    +--------------+






# Instalación

Se apoya en [Docker](https://www.docker.com/) y [fig](http://www.fig.sh/index.html). En el fichero fig-template.yml tienes un ejemplo con varios contenedores. Las imágenes que utilizo las encontrarás aquí: 

* GitHub [base-postfix](https://github.com/LuisPalacios/base-postfix)
* GitHub [base-chatarrero](https://github.com/LuisPalacios/base-chatarrero)
* GitHub [base-courierimap](https://github.com/LuisPalacios/base-courierimap)
* GitHub [base-mysql](https://github.com/LuisPalacios/base-mysql)
* GitHub [base-fluentd](https://github.com/LuisPalacios/base-fluentd)
* GitHub [base-eskibana](https://github.com/LuisPalacios/base-eskibana)

Para instalar y crear los contenedores haz un clone en tu Host

    $  git clone https://github.com/LuisPalacios/servicio-correo.git

Renombra, edita y adapta a tu gusto y necesidades 

    $ mv fig-template.yml fig.yml

Arranca el servicio con el comando siguiente

    $ fig up -d


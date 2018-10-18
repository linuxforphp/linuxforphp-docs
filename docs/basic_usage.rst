.. _BasicUsageAnchor:

Basic Usage
===========

Once Docker is installed, you can now run a *Linux for PHP* container. It is possible to run the container in
Docker's interactive or detached modes.

.. index:: Mode - interactive

Docker's Interactive Mode
-------------------------

To run a *Linux for PHP* container in interactive mode, with the non thread-safe version of PHP 7.2.5, please
enter the following command using your system's shell (BASH/PowerShell)::

    $ docker run --rm -it asclinux/linuxforphp-8.1:7.2.5-nts /bin/bash

You will then get a command-line interface similar to this one :

.. image:: /images/basic_usage01.png

Once you are done with the container, please quit the container by typing::

    $ exit

.. index:: Mode - detached

Docker's Detached Mode
----------------------

And, what if you wish to run a PHP application from a Web browser and start the Linux for PHP container in
Docker's detached mode? To do so, enter the following command::

    # Change to your project's working directory
    $ cd /my/project/folder
    $ docker run -dit --restart=always \
    -v ${PWD}/:/srv/www \``
    -p 8181:80 \
    -p 10443:443 \
    asclinux/linuxforphp-8.1:7.2.5-nts \
    lfphp

.. note:: This last command uses the "lfphp" script to start all available services inside the container. For more details, please see :ref:`lfphp-services`

You should now be able to access any of the PHP scripts contained in your project folder by pointing your browser to `<http://localhost:8181/>`_.

Once you are done with the container, you can stop and remove it as you would any other Docker container running in detached mode::

    $ docker stop [container_id_here]
    $ docker rm [container_id_here]

.. index:: Binaries - pre-compiled PHP

Pre-compiled PHP binaries
-------------------------

As metioned previously, it is possible to run *Linux for PHP* containers that come with pre-compiled binary versions of
any of the major versions of PHP. To obtain a list of the available binaries, please visit the project's download page:

`<https://linuxforphp.net/download>`_

.. index:: Binaries - compiling PHP from source

.. index:: Commands - lfphp-compile

.. _lfphp-compile:

Compiling PHP from source
-------------------------

Of course, *Linux for PHP* is a lightweight version of Linux with all the software needed to easily compile any recent
version of PHP. Thus, if you prefer to compile and use a different version of PHP, you can do so by entering the
following command in a new terminal window. Please make sure to enter the version that you wish to compile, as per the
following example (7.3.0dev in this case)::

    $ docker run -dit -p 8181:80 asclinux/linuxforphp-8.1:src /bin/bash -c "lfphp-compile 7.2.10 nts"

After a few minutes, the new PHP binaries will be compiled and ready to be used! You can always check on the
compilation's progress by connecting to the container using the 'docker exec' command::

    $ docker exec -it [id_of_the_container] /bin/bash

On the container's CLI, enter the 'top' command::

    $ top

To return to the command line, press ``Q``.

.. index:: Binaries - compiling PHP from source manually

Manually compiling PHP from source
----------------------------------

Alternatively, you could also decide to do it manually. If so, start by running a Linux for PHP base image containing
the PHP source files with the following command::

    $ docker run --rm -it asclinux/linuxforphp-8.1:src /bin/bash

And, on the container's command line interface (CLI), checkout the version of PHP you wish to compile and begin
compilation by entering the following commands (in our example, we will compile from master)::

    $ cd /root/php-src
    $ git fetch --all --tags
    $ git pull origin master
    $ ./buildconf --force
    $ ./configure --prefix=/usr \
    --sysconfdir=/etc \
    --localstatedir=/var \
    --datadir=/usr/share/php \
    --mandir=/usr/share/man \
    --enable-fpm \
    --with-fpm-user=apache \
    --with-fpm-group=apache \
    --with-config-file-path=/etc \
    --with-zlib \
    --enable-bcmath \
    --with-bz2 \
    --enable-calendar \
    --enable-dba=shared \
    --with-gdbm \
    --with-gmp \
    --enable-ftp \
    --with-gettext=/usr \
    --enable-mbstring \
    --with-readline \
    --with-mysql-sock=/run/mysqld/mysqld.sock \
    --with-curl \
    --with-openssl \
    --with-openssl-dir=/usr \
    --with-mhash \
    --enable-intl \
    --with-sodium=/usr \
    --with-libxml-dir=/usr \
    --with-libdir=/lib64 \
    --enable-sockets \
    --enable-libxml \
    --enable-soap \
    --with-gd \
    --with-jpeg-dir=/usr \
    --with-png-dir=/usr \
    --with-zlib-dir=/usr \
    --with-freetype-dir=/usr \
    --enable-exif \
    --with-xsl \
    --with-xmlrpc \
    --with-pgsql \
    --with-pdo-mysql=/usr \
    --with-pdo-pgsql \
    --with-mysqli \
    --with-ldap \
    --with-ldap-sasl \
    --enable-opcache
    $ make
    $ make test
    $ make install
    $ install -v -m644 php.ini-production /etc/php.ini
    $ mv -v /etc/php-fpm.conf{.default,}
    $ cp -v /etc/php-fpm.d/www.conf.default /etc/php-fpm.d/www.conf
    $ sed -i 's@php/includes"@&\ninclude_path = ".:/usr/lib/php"@' /etc/php.ini
    $ sed -i -e '/proxy_module/s/^#//' -e '/proxy_fcgi_module/s/^#//' /etc/httpd/httpd.conf
    $ echo 'ProxyPassMatch ^/(.*.php)$ fcgi://127.0.0.1:9000/srv/www/$1' >> /etc/httpd/httpd.conf
    $ sed -i 's/DirectoryIndex index.html/DirectoryIndex index.php index.html/' /etc/httpd/httpd.conf
    $ /etc/init.d/mysql start
    $ /usr/sbin/php-fpm &
    $ /etc/init.d/httpd start

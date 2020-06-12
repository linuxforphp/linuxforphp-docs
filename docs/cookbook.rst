.. _CookbookAnchor:

Linux for PHP Cookbook
======================

Here are a few examples of ways that you can use *Linux for PHP* containers, in order to use them in production
or to debug/profile a slow PHP script you are working on in development.

.. index:: CMS installation

CMS Installation Using the ``lfphp-get`` Command
------------------------------------------------

To install *Concrete5*, *Drupal*, *Joomla* or *WordPress* inside a Linux for PHP container, please enter the following command
on the container's CLI::

    $ lfphp-get cms

After asking which CMS is to be installed and what is the project's name, the ``lfphp-get`` command will set everything up for you. Once it is done,
you will be able to finish installing *Concrete5*, *Drupal*, *Joomla* or *WordPress* by using the corresponding CMS' default
Web installer. By default, the CMS will be installed in the ``/srv/tempo`` folder inside the container. If you share this folder
with the host, you will be able to access the source from outside the container after the end of the CMS' installation.

.. note:: If you are using clean URLs, you might have to create the appropriate .htaccess file according to what is needed to run the CMS that you are installing.

.. index:: Framework installation

.. index:: PHP framework installation

PHP Framework Installation Using the ``lfphp-get`` Command
----------------------------------------------------------

To install *Zend Framework*, *Zend Expressive*, *Symfony*, *Laravel*, *CakePHP*, *Slim* or *LightMVC* inside a Linux for PHP container, please enter the following command
on the container's CLI::

    $ lfphp-get php-frameworks

After asking which framework is to be installed and what is the project's name, the ``lfphp-get`` command will set everything up for you.

.. note:: By default, the framework's skeleton application will be installed in the ``/srv/tempo`` folder inside the container. If you share this folder with the host, you will be able to access the source code from outside the container after the end of the framework's installation.

.. index:: Blackfire.io installation

Blackfire.io Installation Using the ``lfphp-get`` Command
---------------------------------------------------------

If you wish to install and configure *Blackfire.io* on *Linux for PHP*, please run this command::

    $ lfphp-get blackfire

Once done, you will be able to profile your PHP applications by using the *Blackfire* command line tool we installed previously
or by installing and using the *Blackfire* browser plugin in your favorite browser.

.. index:: Node.js installation

Node.js Installation Using the ``lfphp-get`` Command
----------------------------------------------------

If you wish to install and configure *Node.js* on *Linux for PHP*, please run this command on the container's CLI::

    $ lfphp-get nodejs

Once done, *Node.js* and the npm tools will be available from within your *Linux for PHP* container.

.. note:: It is possible to compile *Node.js* from source by adding the ``--compile`` option to the ``lfphp-get`` command.

For more information on the ``lfphp-get`` command, please see :ref:`lfphp-get`.

.. index:: MongoDB installation

MongoDB Installation Using the 'lfphp-get' Command
--------------------------------------------------

If you wish to install and configure *MongoDB* on *Linux for PHP*, please run this command on the container's CLI::

    $ lfphp-get mongodb

Once done, *MongoDB* and its import and export tools will be available from within your *Linux for PHP* container.

If you wish to use the *MongoDB* extension for PHP, you can do so by entering the following commands on the container's CLI::

    $ pecl install mongodb
    $ echo "extension=mongodb.so" >> /etc/php.ini

.. note:: It is possible to compile *MongoDB* from source by adding the ``--compile`` option to the ``lfphp-get`` command.

For more information on the ``lfphp-get`` command, please see :ref:`lfphp-get`.

.. note:: Also, it is possible to use *MongoDB* with all of its SSL options by default.

.. index:: Production - settings and configuration

Configuring PHP with Production Settings
----------------------------------------

.. note:: ATTENTION! This code example does NOT cover security issues and how to harden your server installation!

In order to configure *Linux for PHP* with the most common production settings and extensions, please run an *Linux for PHP* base image
with the PHP source code (asclinux/linuxforphp-8.1:src) with the following command::

    $ docker run -dit -p 8181:80 asclinux/linuxforphp-8.1:src /bin/bash -c "lfphp-compile 7.2.12 nts"

Once done, you will be able to run any PHP script from the CLI or the Web server with the most common production settings.

For more information on the ``lfphp-compile`` command, please see :ref:`lfphp-compile`.

.. index:: Multithreading

.. index:: Thread-safety

.. index:: PHP Extensions - pthreads

.. index:: Posix Threads (pthreads)

Running Multithreaded PHP Scripts
---------------------------------

In order to run a multithreaded PHP script inside a *Linux for PHP* container, please enter the following command::

    $ docker run --rm -it asclinux/linuxforphp-8.1:7.0.29-zts /bin/bash

Then, on the container's CLI, please enter these commands::

    $ pecl install pthreads
    $ echo "extension=pthreads.so" >> /etc/php.ini

After restarting PHP-FPM (if necessary), you will be able to run multithreaded PHP scripts on your computer.

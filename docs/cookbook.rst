.. _CookbookAnchor:

Linux for PHP Cookbook
======================

Here are a few examples of ways that you can use *Linux for PHP* containers, in order to use them in production
or to debug/profile a slow PHP script you are working on in development.

.. index:: CMS installation

CMS Installation Using the ``lfphp-get`` Command
------------------------------------------------

To install *Concrete5*, *Drupal*, *Joomla*, *WordPress*, *Magento* or *Prestashop* inside a Linux for PHP container, please enter the following command
on the container's CLI::

    $ lfphp-get cms

After asking you which CMS you wish to install, and what is your project's name, the ``lfphp-get`` command will set everything up for you. Once this is done,
you will be able to finish installing *Concrete5*, *Drupal*, *Joomla*, *WordPress*, *Magento* or *Prestashop* by using the corresponding CMS' default
Web installer. By default, the CMS will be installed in the ``/srv/tempo`` folder inside the container. If you share this folder
with the host, you will be able to access the source from outside the container after the end of the CMS' installation.

.. note:: If you are using clean URLs, you might have to create the appropriate .htaccess file according to what is needed to run the CMS that you are installing.

.. index:: Framework installation

.. index:: PHP framework installation

PHP Framework Installation Using the ``lfphp-get`` Command
----------------------------------------------------------

To install *Laminas (Zend Framework)*, *Mezzio (Zend Expressive)*, *Symfony*, *Laravel*, *CakePHP*, *Slim* or *LightMVC* inside a Linux for PHP container, please enter the following command
on the container's CLI::

    $ lfphp-get php-frameworks

After asking you which framework you want to install, and what is your project's name, the ``lfphp-get`` command will set everything up for you.

.. note:: By default, the framework's skeleton application will be installed in the ``/srv/tempo`` folder inside the container. If you share this folder with the host, you will be able to access the source code from outside the container after the end of the framework's installation.

.. index:: Blackfire.io installation

Blackfire.io Installation Using the ``lfphp-get`` Command
---------------------------------------------------------

If you wish to install and configure *Blackfire.io* on *Linux for PHP*, please run this command::

    $ lfphp-get blackfire

Once done, you will be able to profile your PHP applications by using the *Blackfire* command line tool directly,
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

    $ lfphp-get --force php-ext mongodb
    $ echo "extension=mongodb.so" >> /etc/php.ini

.. note:: It is possible to compile *MongoDB* from source by adding the ``--compile`` option to the ``lfphp-get`` command.

For more information on the ``lfphp-get`` command, please see :ref:`lfphp-get`.

.. note:: Also, it is possible to use *MongoDB* with all of its SSL options by default.

.. _install:

Installation
============

.. note::

    CEWW doesn't use labeled or numbered releases. The code in the master branch of the repository should be runnable.

Make sure the requirements are satisfied.

The CEWW application is based on Symfony 3.2. Installation follows the normal process for installing a Symfony application.

1. Get the code from GitHub. 

.. code-block:: bash

  git clone https://github.com/sfu-dhil/ceww.git

2. Get the submodules from Git. There is quite a bit of reusable code in the application, and it's organized with git submodules. Navigate into the 'ceww' directory and type:

.. code-block:: bash

  git submodule init
  git submodule update --recursive --remote

3. Create a database and database user. (On Windows you can a server suite like `XAMPP`_ or `WAMP`_-make sure you get the right PHP version)
  
.. code-block:: sql

  create database ceww;
  grant all on ceww.* to ceww@localhost;
  set password for ceww@localhost = password('hotpockets');

Pass the database parameters to the 'parameters.yml' file under the 'ceww/app/config/'.

4. `Install composer`_ if it isn't already installed somewhere. Make sure you do a global installation
  
5. Install the composer dependencies (make sure the working directory is 'ceww'). Composer will ask for some configuration variables during installation.
  
.. code-block:: bash

  composer install --no-dev -o
   
Sometimes composer runs out of memory. If that happens, try this alternate.
  
.. code-block:: bash

  php -d memory_limit=-1 `which composer` install --no-dev -o

6. Update file permissions if needed. The user running the web server must be able to write to `var/cache/*` and `var/logs/*` and `var/sessions/*`. The symfony docs provide `recommended commands`_ depending on your OS.
  
7. Load the schema into the database. This is done with the symfony console.
  
.. code-block:: bash

  ./bin/console doctrine:schema:update --force
  
8. Create an application user with full admin privileges. This is also done with the symfony console.
  
.. code-block:: bash

  ./bin/console fos:user:create admin@example.com
  ./bin/console fos:user:promote admin@example.com ROLE_ADMIN
  

9. At this stage Symfony needs further bundle configurations. In the ceww directory, type:

.. code-block:: bash

  composer require symphony/web-server-bundle

Enable the bundle in the kernel of the application (ceww/app/AppKernel.php). Add the line '$bundles[] = new Symfony\Bunndle\WebServerBundle\WebServerBundle();' as:

.. code-block:: php

  class AppKernel extends Kernel
  {
  // ...
    if ('dev' === $this->getEnvironment()) {
      // ...
      $bundles[] = new Symfony\Bundle\WebServerBundle\WebServerBundle();
      }
  // ...
  }


10. Install bower, npm, and nodejs if you haven't. In the 'ceww' directory, use bower to download and install the javascript and css dependencies.
  
.. code-block:: bash

  bower install

11. Configure the web server. The application's `web/` directory must be accessible to the world. Symfony provides `example configurations`_ for most server setups.


12. start the built-in php server to run the application.

.. code-block:: bash

  php bin/console server:start

Go to the localhost:8000 in your browser.

At this point, the web interface should be up and running, and you should be able to login by following the Login link in the top right menu bar.

The documentation module should be seperately built in and placed inside the 'ceww/web/docs' directory. Follow the `DHIL Documentation Guide`_ for more information.

That should be it.

.. _`XAMPP`: https://www.apachefriends.org/download.html

.. _`WAMP`: http://www.wampserver.com/en/

.. _`Install composer`: https://getcomposer.org/download/

.. _`recommended commands`:
   http://symfony.com/doc/current/setup/file_permissions.html

.. _`example configurations`:
   http://symfony.com/doc/current/setup/web_server_configuration.html

.. _`DHIL Documentation Guide`: https://github.com/sfu-dhil/dhil-docs-guide
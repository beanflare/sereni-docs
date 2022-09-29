Install
==============

Thanks for purchasing `Sereni <https://beanflare.com>`__.

.. NOTE:: Build with `Laravel 9 Framework <https://laravel.com>`__

.. Note:: Requires PHP >= 8.0 and MySQL.

.. ATTENTION:: You can check your server requirements by downloading `this file <https://d3psyma08i51ex.cloudfront.net/sereni/requirements.zip>`__ extract it and upload to your server and access it via a URL i.e http://yourdomain.com/requirements.php (Delete after use).

Required PHP Extensions
^^^^^^^^^^^^^^^^^^^^^^^
- OpenSSL
- PDO
- Mbstring
- Tokenizer
- JSON
- CURL
- Mysqli
- Zip
- GD
  
Release Notes: `Changelogs <changelogs.html>`__ 

Any feature you need to see on Sereni? `request it here <https://support.beanflare.com>`_

Create Database
^^^^^^^^^^^^^^^^^
.. ATTENTION:: Do not use a password that contains a #(Hash) character (It will be treated as a comment)

You’ll need to create a new database along with a user to access it. Most hosting companies provide an interface to handle this or you can run the SQL statements below.

 - Give your database a name e.g **sereni**
 - Create a database user and set up a password
 - Add the user to the database and give the user **All privileges** to the database

.. code-block:: shell

	CREATE DATABASE sereni;  
	CREATE USER 'sereni' IDENTIFIED BY 'sereni@@';  
	GRANT ALL PRIVILEGES ON sereni.* to sereni@'%' identified by 'sereni@@';  
	FLUSH PRIVILEGES;

- You will need to enter this credentials in the steps below.

There are 2 steps to install Sereni App
 - Using the built in web installer
 - Laravel artisan install method (Requires terminal)
   
.. ATTENTION:: Do not modify config files located in /config folder incase you need to modify configuration paramers do it in .env file (Located in application's root folder). When you modify /config files your changes may be overwritten during an update.


Web Installer
^^^^^^^^^^^^^^^
Download the code from Envato Market Downloads page. 
The zipped file includes all dependencies.
The following steps shows how to install Sereni;

The steps below shows how you can install sereni to be accessible via a sub domain e.g **https://portal.yourdomain.com**

 - Create a folder inside **public_html or www** e.g **sereni** and upload your downloaded files into the folder **/public_html/sereni** (Make sure it includes hidden files e.g .env)
 - Go to **sub domains** on your cpanel dashboard and create a sub domain
 - Enter your preferred sub domain e.g portal.yourdomain.com
 - Enter **/public_html/sereni** as your **Document Root**
 - Save it and access **http://portal.yourdomain.com/installer** 
 
 .. ATTENTION:: The url **/installer** does not point to a folder so do not attempt to create a folder named installer
   
If you encounter an issue displaying images refer to troubleshooting tips below.

Installation
""""""""""""""
Once you can access the site the initial setup screen will enable you to configure the database as well as create the initial admin user.
The first page of the web installer checks if your server meets the requirements to run the application.

.. ATTENTION:: Sereni requires PHP 8.0+

Click **Next** if everything is alright if an extension is missing please contact your hosting provider or install it.
The next step checks **directory permissions**. The folders listed should be writable please do NOT set your permissions to **777**.
The next step requires database and account information. 
Enter the information and complete the installation.

.. ATTENTION:: You will need to setup CRON to run every minute as shown below otherwise recurring invoices or tasks will not work.

File Permissions
""""""""""""""""""
The webserver should be able to write to this directories **storage**, **public** and **bootstrap/cache**.
Here is a sample of how you can set the permissions in ubuntu server.

.. code-block:: shell

   sudo chown -R ubuntu:www-data /path/to/sereni
   cd /path/to/sereni
   sudo find -type f -exec chmod 664 {} \;
   sudo find -type d -exec chmod 775 {} \;
   sudo chgrp -R www-data bootstrap/cache storage
   sudo chmod -R ug+rwx bootstrap/cache storage

- Enter your application name and application URL (e.g https://portal.yourdomain.com)
- Enter your database access information that you used when creating database.
- Enter your admin account information. (This is the admin account you are going to login with)
- Click on install and Sereni will perform the migrations and seeding.
- If everything went well, you should get a success screen. Click on **Exit** and login using admin account you created above.
  

.. ATTENTION:: You will need to setup email inorder to verify users accounts. More on that in next article (Configure)


Installing through SSH (Artisan command)
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
If you need to install the app using ``php artisan`` command proceed as follows;
 - Open **.env** file and update your database credentials i.e **DB host,DB User etc** (You can change other configurations later).
 - Run command ``php artisan sereni:install`` to start the installation.
 - You will be asked to enter admin email and password.
 - After successfull install you can now access your dashboard using http://portal.yourdomain.com
 - Use your admin account to login.
  
.. NOTE:: Admin account created using ``php artisan sereni:install`` command does not require email verification.

Email Configuration
^^^^^^^^^^^^^^^^^^^^^

 - Sereni supports SMTP, Mailgun, Postmark, Amazon SES, and sendmail.
 - If you have no idea how to configure email sending, read on the next guide **Configuration**.
 - For more information check https://discuss.sereni.com/

CRON Configuration
^^^^^^^^^^^^^^^^^^^^
Add a CRON job as shown below;

``* * * * * cd /path/to/sereni && php artisan schedule:run >> /dev/null 2>&1``

This Cron will call the command scheduler every minute. When the **schedule:run** command is executed, Sereni will evaluate your scheduled tasks and runs the tasks that are due.

More information available here https://discuss.sereni.com

Queue Configuration (optional)
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. NOTE:: For VPS or AWS EC2 users, we recommend installing Supervisord to monitor your processes. Steps on how to install Supervisor on ubuntu are described below

If you need to use supervisord to monitor your queued jobs follow the steps below;

- Open **app/Console/Kernel.php** and comment the line ``$schedule->command('queue:work --queue=default,high,normal,low --tries=3')....``
- Now install and start supervisor as described below;

Installing Supervisor
"""""""""""""""""""""""
Supervisor is a process monitor for the Linux operating system, and will automatically restart your queue:work process if it fails. To install Supervisor on Ubuntu, you may use the following command:

``sudo apt-get install supervisor``

Supervisor configuration files are typically stored in the **/etc/supervisor/conf.d** directory. Within this directory, you may create any number of configuration files that instruct supervisor how your processes should be monitored. For example, let's create a sereni-worker.conf file that starts and monitors a queue:work process:

.. code-block:: shell

	[program:sereni-worker]
	process_name=%(program_name)s_%(process_num)02d
	command=php /path/to/sereni/artisan queue:work --queue=default,high,normal,low --tries=3
	autostart=true
	autorestart=true
	user=ubuntu
	numprocs=1
	redirect_stderr=true
	stdout_logfile=/path/to/sereni/worker.log

You can refer to `laravel docs <https://laravel.com/docs/9.0/queues#supervisor-configuration>`__ 

Starting Supervisor
""""""""""""""""""""""
Once the configuration file has been created, you may update the Supervisor configuration and start the processes using the following commands:

``sudo supervisorctl reread``

``sudo supervisorctl update``

``sudo supervisorctl restart all``

For more information on Supervisor, consult the Supervisor documentation.


See the `details here <configure.html>`_ for additional configuration options.

Troubleshooting
^^^^^^^^^^^^^^^^^

- Check your webserver log (ie, /var/log/apache2/error.log) and the application logs (storage/logs/laravel-error.log) for more details or set ``APP_DEBUG=true`` in .env
- Getting 404 not found when i access http://portal.mydomain.com/installer - Ensure your sub domain ROOT Document points to /path/to/sereni/public folder and not /path/to/sereni folder.
- I cannot see a folder named **installer** - The url /installer is a laravel route and not a folder. You will be redirected to /installer if the application detects that the app needs to be installed.
- To resolve ``file_put_contents(...): failed to open stream: Permission denied`` run ``chmod -R 777 storage`` then ``chmod -R 755 storage``
- Running ``composer install --no-dev`` and ``composer dump-autoload`` can sometimes help with composer problems.
- Getting error message "Database connection/migration failed" all database credentials are correct. Check that your database user has enough privileges to perform database actions, sereni database should be empty or your password contains a #(Hash).
- Composer install error: ``Fatal error: Allowed memory size of...`` Try the following: ``php -d memory_limit=128M /usr/local/bin/composer install --no-dev``
- My CRONs are not running and i get an error **ErrorException with message 'Invalid argument supplied for foreach()' in /home/project/vendor/symfony/console/Input/ArgvInput.php** to fix this, enter your CRON to run every minute as shown ``php -d register_argc_argv=On /path/to/sereni/artisan schedule:run >/dev/null``

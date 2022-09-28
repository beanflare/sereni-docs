Update
======

.. NOTE:: We recommend backing up your files and database before updating the app.

Sereni checks for updates daily and notifies you via email if there is a new update. You can disable update notifications in **Settings** > **Updates** > **Notify me on new updates**.

.. ATTENTION:: Updates require a valid purchase code. Enter your purchase code in **Settings** > **Updates** and save.

To update your portal go to **Settings > Updates** and click on **Install** button to install available updates.

.. ATTENTION:: Your application will be set to maintenance mode when performing the update and restored after the update process has completed.

If you're moving servers make sure to copy over the .env file.

If incase the system update fails and shows **Maintenance Mode**, run the command ``php artisan up`` manually and check the storage/logs folder for issues.
You will receive a notification if the update is successful/fails.

Fixing Permissions
"""""""""""""""""""""
Never set your folder to 777 permission.
If you give any of your folders 777 permissions, you are allowing any user to read, write and execute any file in that directory. What this means is you have given any user (any hacker or malicious person in the entire world) permission to upload ANY file, virus or any other file, and THEN execute that file.

To fix permissions issue run command ``sudo chown -R my-user:www-data /path/to/your/sereni``
The first **my-user** is name of the user, and the second **www-data** is the name of the group.

Then give both yourself and the webserver permissions as shown;

``sudo find /path/to/your/sereni -type f -exec chmod 664 {} \;``
``sudo find /path/to/your/sereni -type d -exec chmod 775 {} \;``

Then give the webserver the rights to read and write to storage and cache.

``sudo chgrp -R www-data storage bootstrap/cache``  

``sudo chmod -R ug+rwx storage bootstrap/cache``  

Credits https://stackoverflow.com/questions/30639174/how-to-set-up-file-permissions-for-laravel-5-and-others

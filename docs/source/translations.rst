Translations
===================
.. meta::
   :description: Support for multiple languages to avoid any language barrier
   :keywords: projects,invoices,freelancer,tasks,contacts,sereni,codecanyon

Translations are a collection of visual elements in `Sereni <https://beanflare.com>`__ application, like labels, information messages, notifications, alerts, statuses, etc

Translation strings may be defined within JSON files that are placed within the /lang directory. When taking this approach, each language supported by your portal would have a corresponding JSON file within this directory. 
You can follow laravel guide on how to add translation strings `here <https://laravel.com/docs/9.x/localization>`__ 
   
Adding Translations
^^^^^^^^^^^^^^^^^^^^^
If you want to add translations to Sereni;
 - Create a JSON file for your language example ``es.json`` inside /lang folder.
 - Copy contents of english file ``en.json`` and paste it to your new file ``es.json``
 - After copying you can now modify the json line values
 - Lastly register your language in Settings > System Settings > Languages section. For language **code** enter **es** and give it a name e.g **spanish**
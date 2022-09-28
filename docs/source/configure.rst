Configure
=========

You can check .env.example file to see additional settings.

CRON Setup
"""""""""""

Create one CRON that runs **every minute** as follows;

.. code-block:: shell

   * * * * * php /path-to-your-project/artisan schedule:run >/dev/null

Email Queues
""""""""""""
Queues allow you to defer the processing of a time consuming task, such as sending an email, until a later time. Deferring these time consuming tasks drastically speeds up web requests to your application.
You can change the queue driver in your .env file ``QUEUE_DRIVER=database``

.. Note:: You can process the jobs by running ``php artisan queue:work --queue=default,high,normal,low --tries=1``.

Supervisord Configuration
-----------------------------
We highly recommend configuring supervisord to monitor your processes and restart your queue if it fails.  
Read more on `Supervisord Configuration <https://laravel.com/docs/9.0/queues#supervisor-configuration>`__


Application Configuration
"""""""""""""""""""""""""
You can set your application to production by modifying this value in .env file.

- ``APP_ENV`` - Set it to **production**
- ``APP_DEBUG`` - Set it to **false**

System Configuration
""""""""""""""""""""  

Backup Settings
---------------

- ``BACKUP_DISKS`` - Comma separated list of backup disks e.g **local,s3**
- ``BACKUPS_MAIL_ALERT`` - Email to send notifications on successfull backup
- ``BACKUPS_SLACK_WEBHOOK`` - Slack webhook to post backup notifications
- ``BACKUPS_SLACK_CHANNEL`` - Slack channel to post backup notifications. Default blank
 
Email Settings
---------------
 - ``MAIL_DRIVER`` - Email driver. Default **smtp**
 - ``MAIL_HOST`` - Email host e.g **smtp.mailtrap.io**
 - ``MAIL_PORT`` - Outgoing email port e.g **2525**, **587**, or **465**
 - ``MAIL_USERNAME`` - Outgoing email username
 - ``MAIL_PASSWORD`` - Outgoing email password
 - ``MAIL_ENCRYPTION`` - Default null other options **tls**, **null**, or **ssl**

 - ``MAIL_FROM_ADDRESS`` - The email address that sends emails (Your company address)
 - ``MAIL_FROM_NAME`` - Your company name that appears on emails

.. ATTENTION:: To send a test email go to **Settings** -> **System Settings** and click on **Test Email** button.

Paypal Configuration
---------------------
To setup paypal;
 - Login to `Paypal <https://developer.paypal.com>`__ account
 - Create a REST API app
 - Copy Client ID and paste it in your .env file PAYPAL_CLIENT_ID=your-key
 - Copy Secret key and paste in your .env file PAYPAL_CLIENT_SECRET=your-secret
 - At the bottom of the page click Add Webhook
 - Set the webhook URL to https://{YOUR-DOMAIN}/api/v1/paypal/webhook replace {YOUR-DOMAIN} with your actual domain e.g https://portal.domain.com/paypal/webhook
 - Enable events **Payment Authorization created, Payment authorization voided, Payment capture completed, Payment capture denied, Payment Capture pending, Payment capture refunded, Payment capture reversed, Payment order cancelled, Payment order created**
 - After the webhook is created, copy the Webhook ID and paste to your .env file PAYPAL_WEBHOOK_ID=your-webhook-id}
 
 .. ATTENTION:: To enable PayPal Live, open .env file and change PAYPAL_MODE=sandbox to PAYPAL_MODE=live

Stripe Configuration
---------------------
To configure `Stripe <https://dashboard.stripe.com>`__, proceed as follows;

 - Login to your stripe dashboard account
 - Get your API Keys by clicking Developers section
 - Obtain your stripe webhook keys by clicking on Webhooks under Developers section of your stripe dashboard.

Open your .env file in Sereni and modify the values below;

.. code-block:: shell

	STRIPE_KEY={YOUR_STRIPE_PUBLISHABLE_KEY}
	STRIPE_SECRET={YOUR_STRIPE_SECRET_KEY}


Stripe Webhook Configuration
-----------------------------
To handle `Stripe <https://dashboard.stripe.com>`__ webhooks, proceed as follows;
 - Login to your stripe dashboard and click on Developers section.
 - Click Webhooks -> Add Endpoint button
 - Enter webhook URL as https://{YOUR-DOMAIN}/api/v1/stripe/webhook replace {YOUR-DOMAIN} with your actual domain e.g https://portal.domain.com/api/v1/stripe/webhook
 - Enable events **payment_intent.canceled, payment_intent.created,payment_intent.succeeded, payment_intent.processing, payment_intent.payment_failed**
 - Save and copy your Signing Secret and paste in your .env file STRIPE_WEBHOOK_SECRET={YOUR_STRIPE_WEBHOOK_KEY}

Razorpay Configuration
------------------------
To configure `RazorPay <https://dashboard.razorpay.com>`__, proceed as follows;

 - Login to your razorpay dashboard account
 - Get your API Keys by clicking Settings -> API Keys section

Open your .env file in Sereni App and modify the values below;

.. code-block:: shell

	RAZORPAY_KEY={RAZORPAY_KEYID}
	RAZORPAY_SECRET={RAZORPAY_SECRET}

.. ATTENTION:: Create Razorpay webhook and enter webhook URL as https://{YOUR-DOMAIN}/api/v1/razorpay/webhook replace {YOUR-DOMAIN} with your actual domain e.g https://portal.domain.com/api/v1/razorpay/webhook

Mollie Configuration
-------------------------
To configure mollie, proceed as follows;

 - Login to your `Mollie <https://www.mollie.com/dashboard>`__ dashboard account
 - Get your API Keys by clicking on Developers section

Open your .env file in Sereni App and modify the values below;

.. code-block:: shell

	MOLLIE_KEY={MOLLIE_API_KEY}

Google ReCaptcha
"""""""""""""""""""
To enable recaptcha V3, first get your recaptcha key and secret from `Google <https://www.google.com/recaptcha>`__.
Open **.env** file on the ROOT folder and enter your values as shown.

.. code-block:: shell

	RECAPTCHAV3_SITEKEY={your-site-key}
	RECAPTCHAV3_SECRET={your-secret}

Go to **Settings** > **System Settings** then enable **Enable Recaptcha**.


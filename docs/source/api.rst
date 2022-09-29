API
===
.. meta::
   :description: Access your invoices, customers and projects using RESTful API
   :keywords: projects,invoices,freelancer,sereni,tasks,contacts,codecanyon

Sereni provides a RESTful API. Below are the API Endpoints you can use;

Authorization
"""""""""""""
To access the API, you first need to copy your Token. Go to top right corner next to your profile avatar image then click **API Tokens**. Create a token by giving it a name and choose which actions it can perform. Copy and save the Token (It won't be shown again).

.. NOTE:: Insert your Token directly into the authorization header on any API requests. Example below;

.. code-block:: shell

  curl -X GET 
  -H "Authorization: Bearer <bearer>" 
  -H "Content-Type: application/json"
  "https://{endpoint}/api/v1/projects"

Authentication Errors
"""""""""""""""""""""
If the bearer token is invalid or expired you will receive a response with the status code set to **HTTP 401 Unauthorized**.

If the token is valid but you don't have enough scope to perform this request you will receive a response with the status code set to **HTTP 403 Forbidden**.

HTTP-Codes
"""""""""""
Actions and errors yield different HTTP response codes.  
Please have a look at the expected response codes in the following list.

 - ``200`` Request OK
 - ``201`` New resource created
 - ``304`` The resource has not been changed
 - ``400`` The request parameters are invalid
 - ``401`` The bearer token or the provided api key is invalid
 - ``403`` You do not possess the required rights to access this resource.
 - ``404`` The resource could not be found / is unknown.
 - ``415`` The data could not be processed or the accept header is invalid.
 - ``422`` Could not save the entity
 - ``500`` An unexpected condition was encountered
 - ``503`` The server is not available (maintenance work).

HTTP-Headers
""""""""""""
.. ATTENTION:: You must provide the following headers in each request:

+---------------+---------------------+-------------------------+
| Header        | Valid Values        | Description             |
+===============+=====================+=========================+
| Accept        | application/json    | API Format              |
+---------------+---------------------+-------------------------+
| Authorization | Bearer {your-token} | Replace with your Token |
+---------------+---------------------+-------------------------+

Expenses
"""""""""""""""""

``POST`` - /api/v1/expenses
-------------------------------
Create a new expense

.. code-block:: shell

  POST /api/v1/expenses
  Accept: application/json
  Authorization: Bearer {your-token}

Parameters
^^^^^^^^^^

+--------------+----------+------------------------------+
| Name         | Required | Description                  |
+==============+==========+==============================+
| amount       | required | Expense amount e.g 1500.00   |
+--------------+----------+------------------------------+
| category_id  | required | Expense category             |
+--------------+----------+------------------------------+
| expense_date | required | Expense date                 |
+--------------+----------+------------------------------+
| vendor       | required | Vendor name e.g Apple        |
+--------------+----------+------------------------------+
| invoiced     | required | A value 0 or 1               |
+--------------+----------+------------------------------+
| project_id   | optional | Associated project ID if any |
+--------------+----------+------------------------------+

``GET`` - /api/v1/expenses/{id}
--------------------------------
Get expense information

.. code-block:: shell

  GET /api/v1/expenses/{id}
  Accept: application/json
  Authorization: Bearer {your-token}

Sample Response
^^^^^^^^^^^^^^^^
.. code-block:: json

  {
        "type": "expenses",
        "id": "97807926684684288",
        "attributes": {
            "id": "97807926684684288",
            "reference": "EXP-0008",
            "name": "Purchase AWS EC2",
            "amount": 20,
            "currency": "USD",
            "vendor": "Amazon",
            "project": {
                "id": "88528805735567360",
                "name": "Artemis III"
            },
            "category": {
                "id": "87137425234726912",
                "name": "Office Rent"
            },
            "client": {
                "id": "87139674321195008",
                "name": "Elon Musk"
            },
            "user": {
                "id": "87137522655825920",
                "name": "Sarah W",
                "email": "sarah@domain.com"
            },
            "invoiced": 0,
            "is_recurring": 0,
            "is_visible": 1,
            "notes": null,
            "expense_date": "2022-09-09T00:00:00+03:00",
            "created_at": "2022-09-27T22:33:46+03:00",
            "updated_at": "2022-09-27T22:51:33+03:00"
        }
    }


``PUT`` - /api/v1/expenses/{id}
--------------------------------
Update an expense

.. code-block:: shell

  PUT /api/v1/expenses/{id}
  Accept: application/json
  Authorization: Bearer {your-token}

Parameters
^^^^^^^^^^

+--------------+----------+--------------------------------------------+
| Name         | Required | Description                                |
+==============+==========+============================================+
| amount       | required | Expense amount e.g 1500.00                 |
+--------------+----------+--------------------------------------------+
| category_id  | required | Expense category                           |
+--------------+----------+--------------------------------------------+
| expense_date | required | Expense date                               |
+--------------+----------+--------------------------------------------+
| billable     | optional | Whether the expense is billable. Default 1 |
+--------------+----------+--------------------------------------------+
| notes        | optional | Expense notes                              |
+--------------+----------+--------------------------------------------+
| project_id   | required | Associated project ID if any               |
+--------------+----------+--------------------------------------------+
| vendor       | optional | Associated vendor name                     |
+--------------+----------+--------------------------------------------+

``DELETE`` - /api/v1/expenses/{id}
-----------------------------------
Delete an expense

.. code-block:: shell

  DELETE /api/v1/expenses/{id}
  Accept: application/json
  Authorization: Bearer {your-token}

``GET`` - /api/v1/expenses
----------------------------------------
Get a list of all expenses

.. code-block:: shell

  GET /api/v1/expenses
  Accept: application/json
  Authorization: Bearer {your-token}

``GET`` - /api/v1/expenses/{id}/comments
------------------------------------------
Show expense comments

.. code-block:: shell

  GET /api/v1/expenses/{id}/comments
  Accept: application/json
  Authorization: Bearer {your-token}

Payments
"""""""""""""""""

``POST`` - /api/v1/payments
-------------------------------
Create a new payment

.. code-block:: shell

  POST /api/v1/payments
  Accept: application/json
  Authorization: Bearer {your-token}

Parameters
^^^^^^^^^^

+----------------+------------+-----------------------------------------------------+
| Name           | Required   | Description                                         |
+================+============+=====================================================+
| invoice_id     | required   | Invoice ID                                          |
+----------------+------------+-----------------------------------------------------+
| payment_date   | required   | Date when the payment was made                      |
+----------------+------------+-----------------------------------------------------+
| amount         | required   | Amount of payment made                              |
+----------------+------------+-----------------------------------------------------+
| payment_method | required   | Payment method ID                                   |
+----------------+------------+-----------------------------------------------------+
| notes          | optional   | Payment additional notes                            |
+----------------+------------+-----------------------------------------------------+
| currency       | optional   | Payment Currency                                    |
+----------------+------------+-----------------------------------------------------+
| send_email     | optional   | If an email should be sent to client. Default 1     |
+----------------+------------+-----------------------------------------------------+

``GET`` - /api/v1/payments/{id}
--------------------------------
Get payment information

.. code-block:: shell

  GET /api/v1/payments/{id}
  Accept: application/json
  Authorization: Bearer {your-token}

Sample Response
^^^^^^^^^^^^^^^^
.. code-block:: json

  {
        "type": "payments",
        "id": "97281884052131840",
        "attributes": {
            "id": "97281884052131840",
            "reference": "PAY-20220926-0001",
            "method": "Stripe",
            "amount": 32.38,
            "currency": "USD",
            "invoice": {
                "id": "97576297596850176",
                "reference": "INV-20220927-0001"
            },
            "project": {
                "id": "87139846275076096",
                "name": "Artemis"
            },
            "client": {
                "id": "87139674321195008",
                "name": "Elon Musk"
            },
            "notes": null,
            "meta": {
                "orderId": "7U6455882K178700M",
                "status": "COMPLETED",
                "amount": "32.38",
                "currency_code": "USD",
                "invoice_id": "88355381969031168",
                "time": "2022-09-26T08:43:27Z"
            },
            "status": "pending",
            "provider_id": "7U6455882K178700M",
            "payment_date": "2022-10-08T00:00:00+03:00",
            "created_at": "2022-09-26T11:43:28+03:00",
            "updated_at": "2022-09-27T21:32:11+03:00"
        }
    }


``PUT`` - /api/v1/payments/{id}
--------------------------------
Update a payment

.. code-block:: shell

  PUT /api/v1/payments/{id}
  Accept: application/json
  Authorization: Bearer {your-token}

Parameters
^^^^^^^^^^

+----------------+------------+-----------------------------------------------------+
| Name           | Required   | Description                                         |
+================+============+=====================================================+
| invoice_id     | required   | Invoice ID                                          |
+----------------+------------+-----------------------------------------------------+
| payment_date   | required   | Date when the payment was made                      |
+----------------+------------+-----------------------------------------------------+
| amount         | required   | Amount of payment made                              |
+----------------+------------+-----------------------------------------------------+
| payment_method | required   | Payment method ID                                   |
+----------------+------------+-----------------------------------------------------+
| notes          | optional   | Payment additional notes                            |
+----------------+------------+-----------------------------------------------------+
| currency       | optional   | Payment Currency                                    |
+----------------+------------+-----------------------------------------------------+

``DELETE`` - /api/v1/payments/{id}
-----------------------------------
Delete a payment

.. code-block:: shell

  DELETE /api/v1/payments/{id}
  Accept: application/json
  Authorization: Bearer {your-token}

``GET`` - /api/v1/payments
----------------------------------------
Get a list of all payments

.. code-block:: shell

  GET /api/v1/payments
  Accept: application/json
  Authorization: Bearer {your-token}

``GET`` - /api/v1/payments/{id}/comments
------------------------------------------
Show payment comments

.. code-block:: shell

  GET /api/v1/payments/{id}/comments
  Accept: application/json
  Authorization: Bearer {your-token}


Clients
"""""""""""""""""

``POST`` - /api/v1/clients
-------------------------------
Create a new client

.. code-block:: shell

  POST /api/v1/clients
  Accept: application/json
  Authorization: Bearer {your-token}

Parameters
^^^^^^^^^^
+-------------+----------+--------------------------+
| Name        | Required | Description              |
+=============+==========+==========================+
| name        | required | Client Name              |
+-------------+----------+--------------------------+
| email       | required | Client email address     |
+-------------+----------+--------------------------+
| phone       | optional | Client phone number      |
+-------------+----------+--------------------------+
| street      | optional | Street Address           |
+-------------+----------+--------------------------+
| zip_code    | optional | Zip Code                 |
+-------------+----------+--------------------------+
| city        | optional | City                     |
+-------------+----------+--------------------------+
| state       | optional | State                    |
+-------------+----------+--------------------------+
| locale      | optional | Preferred locale         |
+-------------+----------+--------------------------+
| country_id  | required | Country                  |
+-------------+----------+--------------------------+
| tax_number  | optional | Client tax number if any |
+-------------+----------+--------------------------+
| currency_id | required | Preferred currency       |
+-------------+----------+--------------------------+
| notes       | optional | Additional notes         |
+-------------+----------+--------------------------+

``GET`` - /api/v1/clients/{id}
--------------------------------
Get client information

.. code-block:: shell

  GET /api/v1/clients/{id}
  Accept: application/json
  Authorization: Bearer {your-token}

Sample Response
^^^^^^^^^^^^^^^^
.. code-block:: json

  {
        "type": "clients",
        "id": "87264004728295424",
        "attributes": {
            "id": "87264004728295424",
            "reference": "CUST0002",
            "name": "Acme Limited",
            "email": "info@acme.com",
            "currency": {
                "code": "KES",
                "name": "Kenyan Shilling"
            },
            "contact": {
                "id": "87425478868209664",
                "name": "Allen McCain",
                "email": "allen@acme.com"
            },
            "phone": "0790089890",
            "street": "123 Street",
            "city": "Eldoret",
            "state": "CA",
            "zip_code": "50762",
            "country": {
                "code": "KE",
                "name": "Kenya"
            },
            "locale": "en",
            "tax_number": "A-KRA",
            "photo": "/storage/photos/HgRdM8LP1eBgmTJunpQYgTo6Cf7jnSM92iedsN3t.jpg",
            "expense": 0,
            "balance": 0,
            "paid": 0,
            "billing_street": "123 Street",
            "billing_city": "Eldoret",
            "billing_zip": "23423",
            "billing_state": "RV",
            "billing_country": {
                "code": "KE",
                "name": "Kenya"
            },
            "status": "Active",
            "notes": null,
            "created_at": "2022-08-29T19:16:00+03:00",
            "updated_at": "2022-09-28T09:48:46+03:00"
        }
    }


``PUT`` - /api/v1/clients/{id}
--------------------------------
Update client information

.. code-block:: shell

  PUT /api/v1/clients/{id}
  Accept: application/json
  Authorization: Bearer {your-token}

Parameters
^^^^^^^^^^
.. TIP:: Same as the create new client API parameters

``DELETE`` - /api/v1/clients/{id}
-----------------------------------
Delete a client

.. code-block:: shell

  DELETE /api/v1/clients/{id}
  Accept: application/json
  Authorization: Bearer {your-token}

``GET`` - /api/v1/clients
----------------------------------------
Get a list of all clients

.. code-block:: shell

  GET /api/v1/clients
  Accept: application/json
  Authorization: Bearer {your-token}

``GET`` - /api/v1/clients/{id}/projects
----------------------------------------
Show client projects

.. code-block:: shell

  GET /api/v1/clients/{id}/projects
  Accept: application/json
  Authorization: Bearer {your-token}

``GET`` - /api/v1/clients/{id}/payments
------------------------------------------
Show client payments

.. code-block:: shell

  GET /api/v1/clients/{id}/payments
  Accept: application/json
  Authorization: Bearer {your-token}

Projects
"""""""""""""""""

``POST`` - /api/v1/projects
-------------------------------
Create a new projects

.. code-block:: shell

  POST /api/v1/projects
  Accept: application/json
  Authorization: Bearer {your-token}

Parameters
^^^^^^^^^^
+----------------+----------+--------------------------------------------------------------------------+
| Name           | Required | Description                                                              |
+================+==========+==========================================================================+
| name           | required | Project Name                                                             |
+----------------+----------+--------------------------------------------------------------------------+
| client_id      | required | Project client ID                                                        |
+----------------+----------+--------------------------------------------------------------------------+
| start_date     | required | Project start date                                                       |
+----------------+----------+--------------------------------------------------------------------------+
| due_date       | required | Project due date                                                         |
+----------------+----------+--------------------------------------------------------------------------+
| description    | optional | Description                                                              |
+----------------+----------+--------------------------------------------------------------------------+
| hourly_rate    | optional | Hourly rate                                                              |
+----------------+----------+--------------------------------------------------------------------------+
| fixed_price    | optional | Fixed Price. E.g 3400.00                                                 |
+----------------+----------+--------------------------------------------------------------------------+
| notes          | optional | Project Notes                                                            |
+----------------+----------+--------------------------------------------------------------------------+
| color          | required | gray,red,orange,yellow,pink,cyan,blue                                    |
+----------------+----------+--------------------------------------------------------------------------+
| estimate_hours | optional | Project Estimated hours                                                  |
+----------------+----------+--------------------------------------------------------------------------+
| billing_method | optional | ``hourly_staff_rate, hourly_task_rate, hourly_project_rate, fixed_rate`` |
+----------------+----------+--------------------------------------------------------------------------+
| status         | optional | active, on_hold                                                          |
+----------------+----------+--------------------------------------------------------------------------+

``GET`` - /api/v1/projects/{id}
--------------------------------
Get project information

.. code-block:: shell

  GET /api/v1/projects/{id}
  Accept: application/json
  Authorization: Bearer {your-token}

Sample Response
^^^^^^^^^^^^^^^^
.. code-block:: json

  {
        "type": "projects",
        "id": "88528805735567360",
        "attributes": {
            "id": "88528805735567360",
            "reference": "PRO0004",
            "name": "Artemis III",
            "currency": "USD",
            "client": {
                "id": "87139674321195008",
                "name": "Elon Musk"
            },
            "start_date": "2022-10-04T00:00:00+03:00",
            "due_date": "2022-10-08T00:00:00+03:00",
            "hourly_rate": "60.00",
            "fixed_rate": null,
            "progress": 65,
            "estimate_hours": "72.00",
            "billable_time": "85798.00",
            "unbillable_time": "0.00",
            "unbilled": "43254.00",
            "sub_total": 1409.89,
            "total_expenses": 770,
            "billing_method": "hourly_task_rate",
            "status": "active",
            "notes": null,
            "created_at": "2022-09-02T07:01:52+03:00",
            "updated_at": "2022-09-28T08:22:12+03:00"
        }
    }


``PUT`` - /api/v1/projects/{id}
--------------------------------
Update project information

.. code-block:: shell

  PUT /api/v1/projects/{id}
  Accept: application/json
  Authorization: Bearer {your-token}

Parameters
^^^^^^^^^^
.. TIP:: Same as the create new project API parameters

``DELETE`` - /api/v1/projects/{id}
-----------------------------------
Delete project

.. code-block:: shell

  DELETE /api/v1/projects/{id}
  Accept: application/json
  Authorization: Bearer {your-token}

``GET`` - /api/v1/projects
----------------------------------------
Get a list of all projects

.. code-block:: shell

  GET /api/v1/projects
  Accept: application/json
  Authorization: Bearer {your-token}

``GET`` - /api/v1/projects/{id}/boards
------------------------------------------
Show available project task boards

.. code-block:: shell

  GET /api/v1/projects/{id}/boards
  Accept: application/json
  Authorization: Bearer {your-token}


``GET`` - /api/v1/projects/{id}/tasks
----------------------------------------
Show available project tasks

.. code-block:: shell

  GET /api/v1/projects/{id}/tasks
  Accept: application/json
  Authorization: Bearer {your-token}

``GET`` - /api/v1/projects/{id}/expenses
------------------------------------------
Show project expenses

.. code-block:: shell

  GET /api/v1/projects/{id}/expenses
  Accept: application/json
  Authorization: Bearer {your-token}

Tasks
"""""""""""""""""

``POST`` - /api/v1/tasks
-------------------------------
Create a new task

.. code-block:: shell

  POST /api/v1/tasks
  Accept: application/json
  Authorization: Bearer {your-token}

Parameters
^^^^^^^^^^
+-----------------+----------+-----------------------------+
| Name            | Required | Description                 |
+=================+==========+=============================+
| project_id      | required | Project ID                  |
+-----------------+----------+-----------------------------+
| priority_id     | required | Task priority ID            |
+-----------------+----------+-----------------------------+
| name            | required | Task Name                   |
+-----------------+----------+-----------------------------+
| start_date      | optional | Task start date             |
+-----------------+----------+-----------------------------+
| due_date        | optional | Task due date               |
+-----------------+----------+-----------------------------+
| hourly_rate     | optional | Hourly rate e.g 30.00       |
+-----------------+----------+-----------------------------+
| milestone_id    | optional | Milestone ID                |
+-----------------+----------+-----------------------------+
| board_id        | required | Task board ID               |
+-----------------+----------+-----------------------------+
| estimated_hours | optional | Task estimated hours e.g 72 |
+-----------------+----------+-----------------------------+
| description     | optional | Description                 |
+-----------------+----------+-----------------------------+
| visible         | required | Hide or show to client      |
+-----------------+----------+-----------------------------+
| is_recurring    | optional | Default 0                   |
+-----------------+----------+-----------------------------+

``GET`` - /api/v1/tasks/{id}
--------------------------------
Get task information

.. code-block:: shell

  GET /api/v1/tasks/{id}
  Accept: application/json
  Authorization: Bearer {your-token}

Sample Response
^^^^^^^^^^^^^^^^
.. code-block:: json

  {
        "type": "tasks",
        "id": "90750456141320192",
        "attributes": {
            "id": "90750456141320192",
            "project": {
                "id": "88528805735567360",
                "name": "Artemis III"
            },
            "user": {
                "id": "87137522655825920",
                "name": "Sarah W",
                "email": "sarah@acme.com"
            },
            "board": {
                "id": "91217428461260800",
                "name": "Backlog"
            },
            "priority": {
                "id": "87137426274914304",
                "name": "Medium"
            },
            "name": "Setup Network",
            "start_date": "2022-10-04T00:00:00+03:00",
            "due_date": "2022-10-08T00:00:00+03:00",
            "progress": 50,
            "hourly_rate": "45.00",
            "logged_time": 31,
            "estimated_hours": "24.00",
            "description": null,
            "visible": 0,
            "created_at": "2022-09-08T11:09:54+03:00",
            "updated_at": "2022-09-28T08:22:12+03:00"
        }
    }


``PUT`` - /api/v1/tasks/{id}
--------------------------------
Update task information

.. code-block:: shell

  PUT /api/v1/tasks/{id}
  Accept: application/json
  Authorization: Bearer {your-token}

Parameters
^^^^^^^^^^
.. TIP:: Same as the create task API parameters above

``DELETE`` - /api/v1/tasks/{id}
-----------------------------------
Delete a task

.. code-block:: shell

  DELETE /api/v1/tasks/{id}
  Accept: application/json
  Authorization: Bearer {your-token}

``GET`` - /api/v1/tasks
----------------------------------------
Get a list of all tasks

.. code-block:: shell

  GET /api/v1/tasks
  Accept: application/json
  Authorization: Bearer {your-token}

``POST`` - /api/v1/tasks/{id}/duplicate
----------------------------------------
Duplicate a task

.. code-block:: shell

  POST /api/v1/tasks/{id}/duplicate
  Accept: application/json
  Authorization: Bearer {your-token}

Parameters
^^^^^^^^^^
+-------------+----------+----------------------------------------------------+
| Name        | Required | Description                                        |
+=============+==========+====================================================+
| task_id     | required | The task ID                                        |
+-------------+----------+----------------------------------------------------+
| timesheets  | required | Set to 1 to duplicate time entries. 0 means ignore |
+-------------+----------+----------------------------------------------------+
| checklists  | required | Set to 1 to duplicate checklists. 0 means ignore   |
+-------------+----------+----------------------------------------------------+
| comments    | required | Set to 1 to duplicate comments. 0 means ignore     |
+-------------+----------+----------------------------------------------------+
| attachments | required | Set to 1 to duplicate files. 0 means ignore        |
+-------------+----------+----------------------------------------------------+
| assignees   | required | Set to 1 to duplicate team members. 0 means ignore |
+-------------+----------+----------------------------------------------------+

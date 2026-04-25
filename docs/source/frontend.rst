Frontend
========

The frontend for Edulearn uses the Jinja templating engine, with pages written in HTML/CSS. 
Some components use JavaScript to enable interactivity.  
Here's a breakdown of the frontend, page-by-page:  

Navbar/base
-----------
Using Jinja, we put the navbar into a 'base.html' template that is reused across all pages.  
The navbar contains link buttons to the quiz, feedback list, and login pages.  
The navbar is styled to cover the entirety of the top of the screen, and each of the links are styled a little differently:  
* The link to the start quiz form is styled with an orange background so it stands out
* The link to the feedback list has a profile icon (in future, we plan to have individual users be able to view their own feedback)
* The login link is styled plainly - black text on white (with orange text on hover)

Front page
----------

Start Quiz page
---------------

Quiz form
---------

Feedback list
-------------

Feedback detail view
--------------------

Login form
----------
In future, we plan to implement a login form.  
For now, this page simply displays a placeholder form, with some plain HTML text entry boxes.  
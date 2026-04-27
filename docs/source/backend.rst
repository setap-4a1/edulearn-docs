Backend
========

This page will explain different components of the backend (e.g question checking, quiz logic)

Routes
------

frontpage '/' (index.html)
``````````````````````
| The route for our homepage is self-explanatory, taking the user to index.html:
.. code-block:: python

    @app.route('/')
    def home():
        return render_template("index.html")

quiz summaries view '/profile' (profile.html)
`````````````````````````````````````````````
| Our profile route loads the quiz summaries for a given user id (hardcoded to 1) and loads them into the profile.html page.  
.. code-block:: python
    @app.route("/profile", methods=['GET'])
    def profile():
        user_id = 1
        quizzes = DB_query_user_quiz_summaries(user_id)
        return render_template("profile.html", quizzes=quizzes)

Specific profile '/profile/<user_id>'
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
| There is a seperate route for loading a particular user's profile page, but this functionality has not been implemented:
.. code-block:: python

    @app.route("/profile/<user_id>", methods=['GET'])
    def user(user_id):
        return render_template("profile.html", user_id)

API
---
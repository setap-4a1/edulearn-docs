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

quiz summaries view '/feedback' (feedback.html)
`````````````````````````````````````````````
| Our feedback route loads the quiz summaries for a given user id (hardcoded to 1) and loads them into the feedback.html page.  
.. code-block:: python
    @app.route("/feedback", methods=['GET'])
    def feedback():
        user_id = 1
        quizzes = DB_query_user_quiz_summaries(user_id)
        return render_template("feedback.html", quizzes=quizzes)

quiz feedback detail view '/quiz_feedback' (quiz_feedback.html)
```````````````````````````````````````````````````````````````
| text

login form view '/login' (login.html)
`````````````````````````````````````
| text

quiz form view '/quiz' (quiz.html/start_quiz.html)
``````````````````````````````````````````````````
| text

quiz topic selection view '/start_quiz' (start_quiz.html)
`````````````````````````````````````````````````````````
| text

individual question '/question/<int:question_id>' (quiz.html)
`````````````````````````````````````````````````````````````
| text

API submit quiz '/api/submitQuiz'
`````````````````````````````````
| text
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
| The quiz feedback view tries to gather detailed feedback for a specified quiz id.
| We start by fetching feedback ID as provided in the request:
.. code-block:: python

    @app.route('/quiz_feedback')
    def quiz_feedback():
        user_id = 1  
        feed_id = request.args.get('feed_id', type=int)

| If there is an invalid (or no) feedback id, we'll feed an error message to the page:
.. code-block:: python

    if feed_id is None:
        return render_template(
            "quiz_feedback.html",
            summary=None,
            questions=[],
            error="Select a completed quiz from your profile first.",
        )

| We pull the feedback using (link to function subheading here)
.. code-block:: python

    questions = DB_query_quiz_feedback_detail(user_id, feed_id)

| If there exists no feedback for the given ID (the feedback object is empty or doesn't exist), we also give an error message:
.. code-block:: python

    if not questions:
        return render_template(
            "quiz_feedback.html",
            summary=None,
            questions=[],
            error="No feedback found for that quiz attempt.",
        )

| Then we pull all the feedback data and convert it into the formats we want, then pack the data nice and neat:
.. code-block:: python

    total_questions = len(questions)
    correct_count = sum(1 for row in questions if row['is_correct'])
    topic_names = sorted(set(row['topic'] for row in questions))
    topic = topic_names[0] if len(topic_names) == 1 else f"Mixed: {' & '.join(topic_names)}"

    summary = {
        'feed_id': feed_id,
        'topic': topic,
        'total_questions': total_questions,
        'correct_count': correct_count,
        'score_percent': round((correct_count / total_questions) * 100),
    }

| Finally, send our quiz summary off to the quiz feedback template for display:
.. code-block:: python

    return render_template("quiz_feedback.html", summary=summary, questions=questions, error=None)


login form view '/login' (login.html)
`````````````````````````````````````
| This route loads our (very placeholder) login template.
.. code-block:: python

    @app.route('/login', methods=['GET'])
    def login():
        return render_template("login.html")

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
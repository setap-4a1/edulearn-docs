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
| Our quiz route tries to gather a random selection of questions from a specified topic up to a specified limit.
| The topic and limit are expected to come through with the page request. We set a default limit of 5 questions.
.. code-block:: python

    @app.route('/quiz')
    def quiz():
        topic = request.args.get('topic')
        limit = request.args.get('limit', type=int, default=5)

| We show topic selection instead if there's no selected topic;
.. code-block:: python

    if not topic:
        return render_template("start_quiz.html")

| We load our questions from the database using (link to subheading), and 404 if there's no questions;
.. code-block:: python

    db_questions = DB_query_questions_list([topic], limit)
    if not db_questions:
        return jsonify({'error': f'No questions found for topic: {topic}'}), 404

| We turn our database questions into the JSON our template expects using (link to subheading), and then pass it through to the quiz template.
.. code-block:: python

    selected_questions = [transform_db_question(row) for row in db_questions]
    return render_template("quiz.html", questions=selected_questions)

quiz topic selection view '/start_quiz' (start_quiz.html)
`````````````````````````````````````````````````````````
| This route loads the quiz topic/limit selection form.
| Users will select a topic and question limit (3-20) for their quiz.
.. code-block:: python

    @app.route('/start_quiz')
    def start_quiz():
        return render_template("start_quiz.html")

individual question '/question/<int:question_id>' (quiz.html)
`````````````````````````````````````````````````````````````
| This quiz loads the quiz form but with a single question.
| We return 404 if we can't find the question in the database.
| Mostly exists for debugging, but nice to have.
.. code-block:: python

    @app.route('/question/<int:question_id>')
    def question(question_id):
        db_question = DB_query_question_by_id(question_id)
        if db_question is None:
            return "Question not found", 404

        return render_template("quiz.html", questions=[transform_db_question(db_question)])

API
---

submit quiz '/api/submitQuiz'
`````````````````````````````
| Our submit quiz API method tries to save a completed quiz to the database.
| We expect the (link here) to call this when a quiz is finished.
| Our first point of order is to check we have answers:
.. code-block:: python

    @app.route('/api/submitQuiz', methods=['POST'])
    def submit_quiz():
        # Read and validate the JSON payload from the request body
        data = request.get_json(silent=True)
        if not data or 'answers' not in data or not isinstance(data['answers'], list):
            return jsonify({'error': 'Missing or invalid answers payload'}), 400

        answers = data['answers']
        if not answers:
            return jsonify({'error': 'No answers submitted'}), 400

| We make sure to set our placeholder user id too!
| Feedback in the database is associated with a user, so we need this.
.. code-block:: python
    
    user_id = 1

| Then we iterate over our answers, tearing out the data into separate lists.
| We wrap the whole thing in a try, and catch any malformed data to throw errors.
| Once we've pulled the whole thing into lists, we make the query to add the feedback to database.
.. code-block:: python

    try:
        que_id_list = []
        ans_corr_list = []
        user_ans_ind_list = []

        for answer in answers:
            q_id = answer.get('qID')
            selected = answer.get('selected')
            is_correct = answer.get('isCorrect')

            if q_id is None or selected is None or is_correct is None:
                return jsonify({'error': 'Invalid answer object'}), 400

            que_id_list.append(int(q_id))
            user_ans_ind_list.append(int(selected))
            ans_corr_list.append(bool(is_correct))
        
        # Save all the answers as a singular feedback record 
        DB_query_insert_feedback(
            user_id=user_id,
            que_id_list=que_id_list,
            ans_corr_list=ans_corr_list,
            user_ans_ind_list=user_ans_ind_list,
        )
    except (ValueError, TypeError):
        return jsonify({'error': 'Answer values must be valid numbers/booleans'}), 400
    except Exception as exc:
        return jsonify({'error': f'Failed to save quiz feedback: {exc}'}), 500

| We then confirm it didn't go horribly wrong:
.. code-block:: python

    return jsonify({
        'status': 'saved',
        'answerCount': len(answers),
    }), 200

health check 'api/health'
`````````````````````````
| Simple health check - if this still works we know we haven't nuked the entire app!
.. code-block:: python

    @app.route('/api/health')
    def health():
        return jsonify({'status': 'ok', 'message': 'Flask app is running'})

Query helpers
-------------

connector
`````````
| text

Questions
`````````

DB_query_questions_list
^^^^^^^^^^^^^^^^^^^^^^^
| text 

DB_query_question_by_id
^^^^^^^^^^^^^^^^^^^^^^^
| text

DB_query_questions_ans
^^^^^^^^^^^^^^^^^^^^^^
| text

Feedback
````````

DB_query_insert_feedback
^^^^^^^^^^^^^^^^^^^^^^^^
| text
DB_query_user_quiz_summaries
^^^^^^^^^^^^^^^^^^^^^^^^^^^^
| text
DB_query_quiz_feedback_detail
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
| text

Misc helpers
------------

transform_db_question()
```````````````````````
| text
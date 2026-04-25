Frontend
========

| The frontend for Edulearn uses the Jinja templating engine, with pages written in HTML/CSS. 
| Some components use JavaScript to enable interactivity.  
| Here's a breakdown of the frontend, page-by-page:  

Navbar/base
-----------
| Using Jinja, we put the navbar into a 'base.html' template that is reused across all pages.  
| The navbar contains link buttons to the quiz, feedback list, and login pages.  
| The navbar is styled to cover the entirety of the top of the screen, and each of the links are styled a little differently:  
* The link to the start quiz form is styled with an orange background so it stands out
* The link to the feedback list has a profile icon (in future, we plan to have individual users be able to view their own feedback)
* The login link is styled plainly - black text on white (with orange text on hover)

Front page
----------
| The front page has some blurb in a hero section, as well as linking to the start quiz form.
code-block::
    <section class="features-section">
    <h2>Why Choose EduLearn?</h2>
    <div class="features-grid">
        <div class="feature-card">
            <div class="feature-icon">
                <img src="{{ url_for('static', filename='quiz.png') }}" alt="Interactive Quizzes" style="width: 75px; height: auto;">
            </div>
            <h3>Interactive Quizzes</h3>
            <p>Engaging challenges designed to test and reinforce your knowledge quickly.</p>
        </div>
        <div class="feature-card">
            <div class="feature-icon">
                <img src="{{ url_for('static', filename='progress.png') }}" alt="Track Progress" style="width: 65px; height: auto;">
            </div>
            <h3>Track Progress</h3>
            <p>Monitor  your performance over time and see exactly where you can improve.</p>
        </div>
    </div>
</section>
| There are a couple of swanky styled cards

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
| In future, we plan to implement a login form.  
| For now, this page simply displays a placeholder form, with some plain HTML text entry boxes.  
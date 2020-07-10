---
title: Day 5 Notes
layout: note
---

# Day 5 Notes

Challenge for 10-11am:

- Save posts (using pickle) and load again when app loads so posts are retained
- Add better styling (HTML/CSS)
- Add a link on the all posts page to submit a new post
  - Or, put the new post form at the top of the all posts page
- Ask for a username
- Save the date/time with the post (like in my chat server)
  - http://cscamp.artifice.cc/daily-notes/day-2.html (second to last code example)

Python Flask:

- run with command: `python3 -m flask run -h 0.0.0.0 -p 10080`

Python Flask files:

- `python-flask/app.py`

```
from flask import Flask, render_template, request, make_response, redirect

app = Flask('Cyber Social')

# define routes:
# routes are how various addresses are handled by our app

posts = []

@app.route('/')
def home():
    return render_template('home.html')

@app.route('/newpost')
def newpost():
    return render_template('newpost.html')

@app.route('/newpost-submit', methods=["POST"])
def newpost_submit():
    title = request.form["title"]
    body = request.form["body"]
    posts.append(f"{title}: {body}")
    return make_response(redirect("/allposts"))

@app.route('/allposts')
def allposts():
    return render_template('allposts.html', posts=posts)
```

- `python-flask/templates/home.html`

```
<html>

<head>
<title>Cyber Social</title>
</head>

<body>

<h1>Welcome to Cyber Social!</h1>

<h2>The most social site on the web!</h2>

<a href="/newpost">Make new post</a>

<br/>

<a href="/allposts">See all posts</a>


</body>

</html>
```

- `python-flask/templates/newpost.html`

```
<html>
<head>
<title>Cyber Social - New Post</title>
</head>

<body>

<h1>New Post</h1>

<form method="post" action="/newpost-submit">

Title: <input type="text" name="title" size="30"/>
<br/>
<textarea rows="5" name="body" placeholder="Type your post here.">
</textarea>
<br/>
<input type="submit" value="Add Post"/>

</body>
</html>
```

- `python-flask/templates/allposts.html`

```
<html>
<head>
<title>Cyber Social - All Posts</title>
</head>

<body>

<h1>All Posts</h1>

<ul>
{% for p in posts %}
<li>{{ p }}</li>
{% endfor %}
</ul>

<a href="/">Go Home</a>

</body>
</html>
```

---

Python Flask logins

- `python-flask/app.py`:

```
from flask import Flask, render_template, request, make_response, redirect
import random

app = Flask('Cyber Social')

# define routes:
# routes are how various addresses are handled by our app

posts = []
loggedin = {}

@app.route('/')
def home():
    return render_template('home.html')

@app.route('/newpost')
def newpost():
    return render_template('newpost.html')

@app.route('/newpost-submit', methods=["POST"])
def newpost_submit():
    title = request.form["title"]
    body = request.form["body"]
    sessionid = request.cookies.get("sessionid")
    if sessionid in loggedin:
        user = loggedin[sessionid]
    else:
        user = "anonymous"
    posts.append(f"{user} wrote '{title}': {body}")
    return make_response(redirect("/allposts"))

@app.route('/allposts')
def allposts():
    return render_template('allposts.html', posts=posts)


users = {'josh': '123456', 'alice': 'abc123'}

@app.route('/login')
def login():
    return render_template('login.html')

@app.route('/login-submit', methods=["POST"])
def login_submit():
    user = request.form['user']
    password = request.form['password']
    if user in users and users[user] == password:
        response = make_response(redirect('/'))
        sessionid = str(random.randint(0,1000000))
        loggedin[sessionid] = user
        response.set_cookie('sessionid', sessionid)
        return response
    else:
        return "Invalid username or password"
```

- `python-flask/templates/login.html`:

```
<html>
<head>
<title>Cyber Social - Login</title>
</head>

<body>
<h1>Login</h1>

<form method="post" action="/login-submit">
User: <input type="text" name="user" size="20"/>
<br/>
Password: <input type="password" name="password" size="20"/>
<br/>
<input type="submit" value="Log in"/>
</form>

</body>
</html>
```


---
title: CS50x - Week 9 Flask
date: 2024-12-02 15:20:34
tags:
categories: 
- CS50x
---

---

- Some of the most popular include: **Django**, **Pyramid**, and **Flask**.
- 看了两遍 Lecture
- 作业 `Finance` 真的有够难啊，做了六个小时...

---

## Shorts

### Display Minutes

```html
<html>
    <head>
        <title>Current Time in Cambridge</title>
    </head>
    <body>
        The current time in Cambridge is 14:08
    </body>
</html>
```

```python
from flask import Flask
from datetime import datetime
from pytz import timezone

app = Flask(__name__)

@app.route("/")
def time():
    now = datetime.now(timezone('America/New_York'))
    return "The current date and time in Cambridge is {}".format(now)
```

### Initiate

```python
from flask import Flask
app = Flask(__name__)
```

### Def Functions

```python
@app.route("/")
def index():
    return "You are at the index page!"

@app.route("/sample")
def sample():
    return "You are on the sample page!"
```

### Run

```bash
export FLASK_APP=application.py
export FLASK_DEBUG=1
flask run
```

### Pass Data

#### HTTP GET

```python
@app.route("/show/<number>")
def show(number):
    return "You passed in {}".format(number)
```

#### HTTP POST

```python
@app.route("/login", methods=['GET', 'POST'])
def login():
    if not request.form.get("username")
        return apology("must provide username")
```

#### Vary the Behaviours

```python
@app.route("/login", methods=['GET', 'POST'])
def login():
    if request.method == "POST":
        # do one thing
    else:
        # do a different thing
```

### Functions

| 函数              | 描述                                                         | 用法示例                                      |
|-------------------|--------------------------------------------------------------|-----------------------------------------------|
| `url_for()`       | 生成指定视图函数的 URL。                                     | `url_for('index')` 生成 `/` 的 URL。         |
| `redirect()`      | 重定向到指定的 URL。                                        | `return redirect(url_for('login'))`         |
| `session()`       | 用于存储用户会话数据，数据在用户会话期间保持。              | `session['username'] = 'John'`               |
| `render_template()`| 渲染指定的模板文件并返回 HTML 响应。                       | `return render_template('index.html')`      |

---

## Flask

### Routes and Requests

#### Routes

- Routes can dene pages of your web application.
  For example
  - `/`
  - `/about-us`
  - `/register`
  - `/submit`

### Run a Flask Application

```bash
flask run
```

### Flask

- **Listen for requests** to a certain route.
- **Execute Python code** depending on the route requested.
- **Render HTML files** depending on the route requested.

```python
from flask import Flask, render_template

app = Flask(__name__)

@app.route("/") # '@' is a decorator.
def index():
    row = db.execute("SELECT * FROM birthday")
    return render_template("index.html", birthdays=row)
```

### Request Methods

#### GET

![flask_00.png](https://s2.loli.net/2024/12/02/3lSJfX7954RcFzQ.png)

#### POST

![flask_01.png](https://s2.loli.net/2024/12/02/Ro3WABtFHv1pCuK.png)

## Forms

```html
<form action="/" method="post">
    <input type="text" name="email">
    <button type="submit">
        Submit
    </button>
</form>
```

```python
request.form.get("email")
```

### HTML Form Elements

```html
<input type="text">
<input type="number">
<input type="checkbox">
```

```html
<input name="month" type="number" placeholder="Month" min="1" max="12" autocomplete="off" autofocus>
```

## Databases

```python
db.execute("""INSERT INTO birthdays (name, month, day) 
           VALUES (?, ?, ?)
           """, 
           name, 
           month, 
           day)
```

## [Jinja](https://jinja.palletsprojects.com/)

```html
{% extends "layout.html" %}
 
{% block body %}

    <tbody>
        {% for birthday in birthdays %}
            <tr>
                <td>{{ birthday.name }}</td>
                <td>{{ birthday.month }} / {{ birthday.day }}</td>
            </tr>
        {% endfor %}
    </body>

{% endblock %}
```

---

## Lecture

### Pass the Key-Value Pairs

```python
from flask import Flask, render_template, request

app = Flask(__name__)

@app.route("/")
def index():
    if "name" in request.args:
        name = request.args["name"] 
    else:
        name = "world"
    return render_template("index.html", placeholder=name)
```

```python
from flask import Flask, render_template, request

app = Flask(__name__)

@app.route("/")
def index():
    name = request.args.get("name", "world") # For `GET`.
    return render_template("index.html", name=name)
```

### 自适配网页

```html
<meta name="viewport" content="initial-scale=1, width=device-width">
```

### 模板

#### 使用模板

```html
{% extends "layout.html" %}
 
{% block body %}

    <!-- Your Content -->

{% endblock %}
```

#### 创建模板

```html
<!-- layout.html -->

<!-- Your Content -->
    {% block body %} {% endblock %}
<!-- Your Content -->
```

### `POST`

```html
    <form action="/" method="post">
        <input type="text" name="name" autocomplete="off" autofocus placeholder="Name">
        <button type="submit">Greet</button>
    </form>
```

```python
from flask import Flask, render_template, request

app = Flask(__name__)

@app.route("/greet", methods=["GET", "POST"])
def greet():
    name = request.form.get("name", "world") # For `POST`.
    return render_template("greet.html", name=name)
```

### 整合到一个函数

```html
<!-- index.html -->

    <form action="/" method="post"> <!-- `action` can be ignore -->
        <input type="text" name="name" autocomplete="off" autofocus placeholder="Name">
        <button type="submit">Greet</button>
    </form>
```

```html
<!-- greet.html -->

    hello {{ name }}
```

```python
from flask import Flask, render_template, request

app = Flask(__name__)

@app.route("/", methods=["GET", "POST"])
def index():
    if request.method == "POST":
        name = request.form.get("name", "world") # For `POST`.
        return render_template("greet.html", name=name)
    return render_template("index.html")
```

- 如果输入了空的东西呢？ `"world"` 其实没有用了。

```python
from flask import Flask, render_template, request

app = Flask(__name__)

@app.route("/", methods=["GET", "POST"])
def index():
    if request.method == "POST":
        name = request.form.get("name")
        return render_template("greet.html", name=name)
    return render_template("index.html")
```

- 检查是否空白。

```html
<!-- greet.html -->

    hello {% if name %}{{ name }}{% else %}world{% endif %}
```

### Dropdown Menu

```html
<!-- index.html -->

    <select name="sport">
        <option disabled selected value="">Sport</option>
        <option>Basketball</option>
        <option>Football</option>
        <option>Ultimate Frisbee</option>
    </select>
```

### 输入框必须含有输入

```html
<!-- index.html -->

    <input required>
```

### 检查是否非空

```python
from flask import Flask, render_template, request

app = Flask(__name__)

@app.route("/")
def index():
    return render_template("index.html")

@app.route("/register", methods=["POST"])
def register():
    if not request.form.get("name") or not request.form.get("sport"):
        return render_template("failure.html")
    return render_template("success.html")
```

### 服务端验证数据

```python
from flask import Flask, render_template, request

app = Flask(__name__)

SPORTS = ["Basketball", "Football", "Ultimate Frisbee"]

@app.route("/")
def index():
    return render_template("index.html",sports=SPORTS)

@app.route("/register", methods=["POST"])
def register():
    if not request.form.get("name") or request.form.get("sport") not in SPORTS:
        return render_template("failure.html")
    return render_template("success.html")
```

```html
<!-- index.html -->

    <select name="sport">
        <option disabled selected value="">Sport</option>
        {% for sport in sports %}
            <option value="{{ sport }}">{{ sport }}</option>
        {% endfor %}
    </select>
```

### Type `radio` (exclusive)

```html
<!-- index.html -->

    {% for sport in sports %}
        <input name="sport" type="radio" value="{{ sport }}">{{ sport }}
    {% endfor %}
```

### Type `checkbox` (inclusive)

```html
<!-- index.html -->

    {% for sport in sports %}
        <input name="sport" type="checkbox" value="{{ sport }}">{{ sport }}
    {% endfor %}
```

- 需要修改程序代码

```python
from flask import Flask, render_template, request

app = Flask(__name__)

SPORTS = ["Basketball", "Football", "Ultimate Frisbee"]

@app.route("/")
def index():
    return render_template("index.html",sports=SPORTS)

@app.route("/register", methods=["POST"])
def register():
    if not request.form.get("name")
        return render_template("failure.html")
    for sport in request.form.getlist("sport"):
        if sport not in SPORTS:
            return render_template("failure.html")
    return render_template("success.html")
```

### 展示注册信息 Using `SQL`

```html
<!-- index.html -->
{% extends "layout.html" %}
 
{% block body %}
    <h1>Registrants</h1>
    <table>
        <thead>
            <tr>
                <th>Name</th>
                <th>Sport</th>
                <th></th>
            </tr>
        </thead>
        <tbody>
            {% for registrant in registrants %}
                <tr>
                    <td>{{ registrant.name }}</td>
                    <td>{{ registrant.sport }}</td>
                    <td>
                        <form action="/deregister" method="post">
                            <input name="id" type="hidden" value="{{ registrant.id }}">
                            <button type="submit">Deregister</button>
                        </form>
                    </td>
                </tr>
            {% endfor %}
        </tbody>
    </table>
{% endblock %}
```

```python
# Implements a registration form, storing registrants in a SQLite database, with support for deregistration
 
from cs50 import SQL
from flask import Flask, redirect, render_template, request
 
app = Flask(__name__)
 
db = SQL("sqlite:///froshims.db")
 
SPORTS = [
    "Basketball",
    "Soccer",
    "Ultimate Frisbee"
]
 
 
@app.route("/")
def index():
    return render_template("index.html", sports=SPORTS)
 
 
@app.route("/deregister", methods=["POST"])
def deregister():
 
    # Forget registrant
    id = request.form.get("id")
    if id:
        db.execute("DELETE FROM registrants WHERE id = ?", id)
    return redirect("/registrants")
 
 
@app.route("/register", methods=["POST"])
def register():
 
    # Validate submission
    name = request.form.get("name")
    sport = request.form.get("sport")
    if not name or sport not in SPORTS:
        return render_template("failure.html")
 
    # Remember registrant
    db.execute("INSERT INTO registrants (name, sport) VALUES(?, ?)", name, sport)
 
    # Confirm registration
    return redirect("/registrants")
 
 
@app.route("/registrants")
def registrants():
    registrants = db.execute("SELECT * FROM registrants")
    return render_template("registrants.html", registrants=registrants)
```

### Cookies

```python
from flask import Flask, redirect, render_template, request, session
from flask_session import Session
 
# Configure app
app = Flask(__name__)
 
# Configure session
app.config["SESSION_PERMANENT"] = False
app.config["SESSION_TYPE"] = "filesystem"
Session(app)
 
 
@app.route("/")
def index():
    return render_template("index.html", name=session.get("name"))
 
 
@app.route("/login", methods=["GET", "POST"])
def login():
    if request.method == "POST":
        session["name"] = request.form.get("name") # 实际上是 `dictionary`.
        return redirect("/")
    return render_template("login.html")
 
 
@app.route("/logout")
def logout():
    session.clear()
    return redirect("/")
```

```html
<!-- index.html -->

{% extends "layout.html" %}
 
{% block body %}
 
    {% if name %}
        You are logged in as {{ name }}. <a href="/logout">Log out</a>.
    {% else %}
        You are not logged in. <a href="/login">Log in</a>.
    {% endif %}
 
{% endblock %}
```

```html
<!-- login.html -->

{% extends "layout.html" %}
 
{% block body %}
 
    <form action="/login" method="post">
        <input autocomplete="off" autofocus name="name" placeholder="Name" type="text">
        <button type="submit">Log In</button>
    </form>
 
{% endblock %}
```

### Safe Wildcard Searches

```python
q = request.args.get("q")
shows = db.execute("SELECT * FROM shows WHERE title LIKE ?", "%" + q + "%")
```

### Interactive `AJAX`

```js
let input = document.querySelector('input');
input.addEventListener('input', async function() {
    let response = await fetch('/search?q=' + input.value);
    let shows = await response.text();
    document.querySelector('ul').innerHTML = shows;
});
```

### API (Application Programming Interface)

### JSON

- Using double quotes but not single quotes in Python `list`.
- Machine readable.

```python
# Searches for shows using Ajax with JSON
 
from cs50 import SQL
from flask import Flask, jsonify, render_template, request
 
app = Flask(__name__)
 
db = SQL("sqlite:///shows.db")
 
 
@app.route("/")
def index():
    return render_template("index.html")
 
 
@app.route("/search")
def search():
    q = request.args.get("q")
    if q:
        shows = db.execute("SELECT * FROM shows WHERE title LIKE ? LIMIT 50", "%" + q + "%")
    else:
        shows = []
    return jsonify(shows)

```

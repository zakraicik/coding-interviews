# Simple Flask Web App

This README provides instructions on how to build and run a simple Python web application using Flask.

## Prerequisites

- Python 3.7 or higher
- pip (Python package installer)

## Setup

1. Create a new directory for your project:

   ```
   mkdir simple-flask-app
   cd simple-flask-app
   ```

2. Create a virtual environment (optional but recommended):

   ```
   python -m venv venv
   source venv/bin/activate  # On Windows, use `venv\Scripts\activate`
   ```

3. Install Flask:

   ```
   pip install Flask
   ```

4. Create a new file named `app.py` with the following content:

   ```python
   from flask import Flask

   app = Flask(__name__)

   @app.route('/')
   def hello_world():
       return 'Hello, World!'

   if __name__ == '__main__':
       app.run(debug=True)
   ```

## Running the Application

1. Run the Flask application:

   ```
   python app.py
   ```

2. Open a web browser and navigate to `http://127.0.0.1:5000/`

You should see "Hello, World!" displayed in your browser.

## Extending the Application

To add more routes and functionality to your web app, you can modify the `app.py` file. For example:

```python
from flask import Flask, render_template

app = Flask(__name__)

@app.route('/')
def hello_world():
    return 'Hello, World!'

@app.route('/greet/<name>')
def greet(name):
    return f'Hello, {name}!'

if __name__ == '__main__':
    app.run(debug=True)
```

This adds a new route that greets a user by name when they visit `/greet/<name>`.

## Server-Side vs. Client-Side Code

In web development, it's important to understand the distinction between server-side and client-side code. Let's illustrate this using our Flask app as an example.

### Server-Side Code

Server-side code runs on the web server and generates the content that is sent to the client's browser. In our Flask app, all the Python code in `app.py` is server-side. For example:

```python
@app.route('/greet/<name>')
def greet(name):
    return f'Hello, {name}!'
```

This code runs on the server when a user visits the `/greet/<name>` URL. The server processes the request, generates the response, and sends it back to the client.

Characteristics of server-side code:

- Executes on the web server
- Can access server resources (databases, file systems, etc.)
- Not visible to the end-user
- Used for processing data, authentication, and generating dynamic content

### Client-Side Code

Client-side code runs in the user's web browser. This typically includes HTML, CSS, and JavaScript. While our basic Flask app doesn't include client-side code yet, let's add a simple example:

1. Create a new directory named `templates` in your project folder.
2. Create a file named `index.html` in the `templates` directory with the following content:

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Flask App</title>
  </head>
  <body>
    <h1>Welcome to our Flask App</h1>
    <p>Server time: <span id="server-time">{{ server_time }}</span></p>
    <p>Browser time: <span id="browser-time"></span></p>

    <script>
      // This is client-side JavaScript
      function updateBrowserTime() {
        document.getElementById('browser-time').textContent =
          new Date().toLocaleString()
      }
      setInterval(updateBrowserTime, 1000)
    </script>
  </body>
</html>
```

3. Update your `app.py` to render this template:

```python
from flask import Flask, render_template
from datetime import datetime

app = Flask(__name__)

@app.route('/')
def index():
    return render_template('index.html', server_time=datetime.now().strftime('%Y-%m-%d %H:%M:%S'))

if __name__ == '__main__':
    app.run(debug=True)
```

In this example:

- The server-side code (Python) generates the initial HTML and provides the server time.
- The client-side code (JavaScript) updates the browser time every second without needing to reload the page or contact the server.

Characteristics of client-side code:

- Executes in the user's web browser
- Can respond to user actions without contacting the server
- Visible to the end-user (they can view the source)
- Used for interactive features, real-time updates, and improving user experience

By understanding the difference between server-side and client-side code, you can create more dynamic and responsive web applications.

## Next Steps

- Add more complex HTML templates using the `render_template` function
- Implement form handling
- Connect to a database
- Add user authentication

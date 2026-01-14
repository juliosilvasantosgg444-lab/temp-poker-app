import os

def create_xit_painel():
    """Creates a functional XIT panel with all necessary functions."""
    
    # Create base directory structure
    dirs = ['app', 'templates', 'static', 'models', 'views', 'utils']
    for d in dirs:
        os.makedirs(f'xit_panel/{d}', exist_ok=True)
    
    # Main app file
    with open('xit_panel/app.py', 'w') as f:
        f.write("""
from flask import Flask, render_template, request
from models import db
from views import dashboard, users, settings

app = Flask(__name__)
app.config['SECRET_KEY'] = 'your-secret-key'
db.init_app(app)

@app.route('/')
def index():
    return render_template('dashboard.html')

if __name__ == '__main__':
    app.run(debug=True)
        """)
    
    # Models
    with open('xit_panel/models/__init__.py', 'w') as f:
        f.write("""
from flask_sqlalchemy import SQLAlchemy

db = SQLAlchemy()

class User(db.Model):
    id = db.Column(db.Integer, primary_key=True)
    username = db.Column(db.String(80), unique=True, nullable=False)
    email = db.Column(db.String(120), unique=True, nullable=False)
        """)
    
    # Views
    with open('xit_panel/views/__init__.py', 'w') as f:
        f.write("""
from flask import Blueprint

dashboard = Blueprint('dashboard', __name__)
users = Blueprint('users', __name__)
settings = Blueprint('settings', __name__)

@dashboard.route('/dashboard')
def dashboard_view():
    return render_template('dashboard.html')

@users.route('/users')
def users_view():
    return render_template('users.html')
        """)
    
    # Templates
    with open('xit_panel/templates/base.html', 'w') as f:
        f.write("""
<!DOCTYPE html>
<html>
<head>
    <title>XIT Panel</title>
    <link rel="stylesheet" href="{{ url_for('static', filename='style.css') }}">
</head>
<body>
    <div class="container">
        {% block content %}{% endblock %}
    </div>
</body>
</html>
        """)
    
    # Static files
    with open('xit_panel/static/style.css', 'w') as f:
        f.write("""
body {
    font-family: Arial, sans-serif;
    margin: 0;
    padding: 20px;
}
.container {
    max-width: 1200px;
    margin: 0 auto;
}
        """)
    
    print("XIT Panel created successfully!")

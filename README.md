# To-Do App

A simple task-tracking web application built with Django. It allows users to create tasks, view all tasks on a single page, edit task details, mark tasks as completed, and delete tasks when they are no longer needed. All task data is stored in a local SQLite database, so no additional database setup is required.

## What It Can Do

- Display all tasks in a table with their title, description, completion checkbox, status, creation date, and action buttons
- Create a new task with a title and description
- Edit the title and description of an existing task
- Delete a task
- Mark a task as **Done** or **Pending** using a checkbox
- Automatically save the task status when the checkbox is clicked
- Display a **"No Task Yet"** message when there are no tasks
- Store all task information in a local SQLite database

## Built With

- Python 
- Django 6.1
- SQLite
- Django Templates
- HTML
- Plain CSS with no frontend frameworks

## Project Structure

```text
manage.py                     # Django command-line utility
db.sqlite3                    # SQLite database used to store tasks

todo_project/                 # Django project configuration
├── __init__.py
├── settings.py               # Project settings
├── urls.py                   # Main URL configuration
├── wsgi.py                   # WSGI entry point
└── asgi.py                   # ASGI entry point

todo_app/                     # Django app containing the to-do functionality
├── __init__.py
├── admin.py                  # Admin configuration
├── apps.py                   # App configuration
├── models.py                 # todo_table model
├── views.py                  # Application views
├── urls.py                   # App-level URL routes
├── tests.py                  # Tests
└── migrations/
    ├── __init__.py
    └── 0001_initial.py       # Initial database migration

templates/
├── home.html                 # Task list page
├── add_task.html             # Add-task form
├── edit_task.html            # Edit-task form
└── navbar.html               # Shared navigation bar

static/css/
├── home.css                  # Styles for the task list
├── add_task.css              # Styles for add/edit forms
└── navbar.css                # Styles for the navigation bar
```

All of these files are inside the `Assignment_to_do/` folder, which is the main project folder containing `manage.py`.

## How the Code Works

### The Model

The application stores task information using a single model called `todo_table`, defined in `todo_app/models.py`.

| Field | Type | Purpose |
|---|---|---|
| Title | `CharField(max_length=50)` | Stores the name of the task |
| Description | `CharField(max_length=100)` | Stores additional details about the task |
| Done | `BooleanField(default=False)` | Stores whether the task is completed |
| Created | `DateTimeField(auto_now_add=True)` | Automatically records when the task was created |

### The Routes

All application routes are defined in `todo_app/urls.py`. The main `urls.py` also provides access to Django's built-in `/admin/` page.

| Path | View | URL name | What it does |
|---|---|---|---|
| `/` | `home` | `home` | Displays all tasks |
| `/add/` | `add_task` | `add_task` | Displays the form and creates a new task |
| `/edit/<id>/` | `edit_task` | `edit_task` | Displays the form and updates an existing task |
| `/delete/<id>/` | `delete_task` | `delete_task` | Deletes a task |
| `/complete/<id>/` | `task_completed` | `task_completed` | Marks a task as done or pending |

### The Views

The functions in `todo_app/views.py` handle the main functionality of the application.

- `home` retrieves all tasks from the database and sends them to `home.html`.
- `add_task` displays an empty form when the page is opened normally. When the form is submitted, it creates a new task and redirects the user to the home page.
- `edit_task` finds a task using its ID, displays its current information in the form, and saves the updated title and description when the form is submitted.
- `delete_task` removes the selected task from the database and redirects the user to the home page.
- `task_completed` receives the checkbox form submission and updates the task's `Done` value. A checked checkbox sets it to `True`, while an unchecked checkbox sets it to `False`.

### The Pages and Styles

`navbar.html` contains the common navigation section with the **"To-Do App"** heading and **Add Task** button. The other pages include this file at the top.

`home.html` displays all tasks in a table. It loops through the tasks and creates one row for each task. Every row contains the task details, completion checkbox, status, creation date, and **Edit** and **Delete** buttons.

The completion checkbox is placed inside a small form. The form is automatically submitted whenever the checkbox is changed, allowing the task status to be updated without requiring a separate submit button.

`add_task.html` contains the form for creating a new task, while `edit_task.html` contains the form for updating an existing task. Both pages use `add_task.css` for styling.

The project uses `home.css` for the task list, `add_task.css` for the add/edit forms, and `navbar.css` for the navigation bar.

In `settings.py`, the `templates/` directory is registered as the template folder using `TEMPLATES["DIRS"]`, and the `static/` directory is registered using `STATICFILES_DIRS`.

## Running the Project on a Mac

You'll need Python 3 installed. Check your Python version using:

```bash
python3 --version
```

If Python is not installed, you can install it using Homebrew:

```bash
brew install python
```

Open Terminal and navigate to the project folder — the folder that contains `manage.py`:

```bash
cd ~/Downloads/Assignment_to_do
```

Create and activate a virtual environment:

```bash
python3 -m venv venv
source venv/bin/activate
```

If the project already contains a `venv` folder and it does not work correctly, remove it and create a new one:

```bash
rm -rf venv
python3 -m venv venv
source venv/bin/activate
```

Install Django:

```bash
pip install django
```

Set up the database. Since the migration file is already included, you only need to run:

```bash
python manage.py migrate
```

Start the development server:

```bash
python manage.py runserver
```

Open the application in your browser:

```text
http://127.0.0.1:8000/
```

Press **Control + C** in the Terminal to stop the server. When you're finished, exit the virtual environment using:

```bash
deactivate
```

If port `8000` is already being used, you can start the server on another port:

```bash
python manage.py runserver 8080
```

Then open:

```text
http://127.0.0.1:8080/
```

## Notes

- `DEBUG = True` and the development `SECRET_KEY` are intended only for local development. They should be changed before deploying the application.
- The project timezone is set to `Asia/Kolkata`.
- The current delete functionality uses a normal `GET` request. For a more secure implementation, deleting a task should ideally be handled through a `POST` request with CSRF protection.
- In `edit_task.html`, the title input should have the Django variable inside quotation marks so titles containing spaces are displayed correctly. For example:

```html
<input type="text" value="{{ task_data.Title }}">
```

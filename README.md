# To-Do App (Django)

A simple task manager built with Django. You can add tasks, view them in a table, edit them, mark them as done with a checkbox, and delete them. Data is stored in a local SQLite database.

## Features

- **View all tasks** in a table (title, description, done checkbox, status, created date, actions)
- **Add** a new task (title + description)
- **Edit** an existing task
- **Delete** a task
- **Mark complete / incomplete** by ticking the checkbox (submits automatically)
- Shows a "No Task Yet" row when the list is empty

## Tech Stack

- Python / Django 6.1
- HTML templates with Django template language
- Plain CSS (no frameworks)

## Project Structure

```
todo_project/                 # Django project (configuration)
├── __init__.py
├── settings.py               # Project settings
├── urls.py                   # Root URL config (admin + includes todo_app.urls)
├── wsgi.py                   # WSGI entry point
└── asgi.py                   # ASGI entry point

todo_app/                     # Django app (the actual to-do logic)
├── __init__.py
├── admin.py                  # (empty) admin registrations
├── apps.py                   # App config (TodoAppConfig)
├── models.py                 # todo_table model
├── views.py                  # home, add, edit, delete, complete views
├── urls.py                   # App-level routes
└── tests.py                  # (empty) tests

templates/
├── home.html                 # Task list page
├── add_task.html             # Add-task form
├── edit_task.html            # Edit-task form
└── navbar.html               # Shared header, included in other pages

static/css/
├── home.css                  # Styles for the task list
├── add_task.css              # Styles for add/edit forms
└── navbar.css                # Styles for the header
```

## How It Works

### Data model (`models.py`)

A single model, `todo_table`:

| Field         | Type                      | Description                          |
|---------------|---------------------------|--------------------------------------|
| `Title`       | `CharField(max_length=50)`  | Task title                         |
| `Description` | `CharField(max_length=100)` | Task description                   |
| `Done`        | `BooleanField(default=False)` | Whether the task is completed    |
| `Created`     | `DateTimeField(auto_now_add=True)` | Set automatically on creation |

### URL routes (`todo_app/urls.py`)

| URL                      | View            | Name            | Purpose                      |
|--------------------------|-----------------|-----------------|------------------------------|
| `/`                      | `home`          | `home`          | List all tasks               |
| `/add/`                  | `add_task`      | `add_task`      | Show/submit the add form     |
| `/edit/<id>/`            | `edit_task`     | `edit_task`     | Show/submit the edit form    |
| `/delete/<id>/`          | `delete_task`   | `delete_task`   | Delete a task                |
| `/complete/<id>/`        | `task_completed`| `task_completed`| Toggle the done status       |


### Views (`views.py`)

- **`home`** – fetches all tasks and renders `home.html`.
- **`add_task`** – on GET shows the form; on POST creates a task from the submitted title and description, then redirects to home.
- **`edit_task`** – loads a task by ID; on POST updates the title and description (only if both are filled in), saves, and redirects home.
- **`delete_task`** – deletes the task and redirects home.
- **`task_completed`** – on POST, sets `Done` to `True` if the checkbox value is `"True"`, otherwise `False`, then redirects home.

### Templates and styling

- `navbar.html` is included at the top of every page and has the "To-Do App" title and an **Add Task** button.
- `home.html` loops over tasks and shows each in a table row, with Edit/Delete buttons and a checkbox that auto-submits a small form when changed.
- `add_task.html` and `edit_task.html` share `add_task.css`.
- Settings point Django to the `templates/` folder (`TEMPLATES["DIRS"]`) and the `static/` folder (`STATICFILES_DIRS`).

## Getting Started

1. **Install Django**
   ```bash
   pip install django
   ```

2. **Apply database migrations**
   ```bash
   python manage.py makemigrations todo_app
   python manage.py migrate
   ```

3. **Run the development server**
   ```bash
   python manage.py runserver
   ```

4. Open <http://127.0.0.1:8000/> in your browser.

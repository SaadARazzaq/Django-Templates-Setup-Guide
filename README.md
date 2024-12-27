# Templates Setup in Django

![image](https://github.com/user-attachments/assets/03ec705c-90c0-4d2d-bb1e-766b14a03fdf)

Follow these steps to set up templates in a Django project. Below, we'll use a mini project example called `myproject` with an app named `myapp` to demonstrate the setup.

## Steps to Set Up Templates

### 1. Create a New Django Project and App

- Start by creating a new Django project:
  ```bash
  django-admin startproject myproject
  cd myproject
  ```

- Create a new app:
  ```bash
  python manage.py startapp myapp
  ```

### 2. Render Templates in Views

- Open `myapp/views.py` and use the `render` function:

  **Without Passing a Dictionary:**
  ```python
  from django.shortcuts import render

  def home_view(request):
      return render(request, 'home.html', {})
  ```

  **With Passing a Dictionary:**
  ```python
  from django.shortcuts import render

  def home_view(request):
      context = {
          'title': 'Home Page',
          'message': 'Welcome to MyProject!'
      }
      return render(request, 'home.html', context)
  ```

### 3. Create Template Files

- Inside the `myproject` directory, create a folder named `templates`.
  ```bash
  mkdir templates
  ```

- Inside the `templates` folder, create a file named `home.html`:
  ```html
  <!DOCTYPE html>
  <html>
  <head>
      <title>{{ title }}</title>
  </head>
  <body>
      <h1>{{ message }}</h1>
  </body>
  </html>
  ```

### 4. Configure `DIRS` in `settings.py`

- Open `myproject/settings.py` and update the `TEMPLATES` configuration:
  ```python
  TEMPLATES = [
      {
          'BACKEND': 'django.template.backends.django.DjangoTemplates',
          'DIRS': [BASE_DIR / 'templates'],  # Add your templates directory here
          'APP_DIRS': True,
          'OPTIONS': {
              'context_processors': [
                  'django.template.context_processors.debug',
                  'django.template.context_processors.request',
                  'django.contrib.auth.context_processors.auth',
                  'django.contrib.messages.context_processors.messages',
              ],
          },
      },
  ]
  ```

### 5. Register the App

- Add the name of your app, `myapp`, to the `INSTALLED_APPS` list in `settings.py`:
  ```python
  INSTALLED_APPS = [
      ...
      'myapp',
  ]
  ```

### 6. Define URL Paths

- Open `myproject/urls.py` and include the app's URLs:
  ```python
  from django.contrib import admin
  from django.urls import path
  from myapp import views

  urlpatterns = [
      path('admin/', admin.site.urls),
      path('', views.home_view, name='home'),
  ]
  ```

### 7. Run the Server

- Start the development server to test your setup:
  ```bash
  python manage.py runserver
  ```

- Open your browser and navigate to `http://127.0.0.1:8000/` to see your `home.html` template rendered.

### 8. Organize Templates by App (Optional)

- For better organization, create a `templates` folder inside the `myapp` directory:
  ```bash
  mkdir myapp/templates/myapp
  ```

- Move your `home.html` file into the `myapp/templates/myapp` directory.

- Update the `render` function in `myapp/views.py` to use the app-specific template:
  ```python
  def home_view(request):
      context = {
          'title': 'Home Page',
          'message': 'Welcome to MyProject!'
      }
      return render(request, 'myapp/home.html', context)
  ```

### 9. Use Template Inheritance

- Create a base template file `base.html` in the `templates` folder:
  ```html
  <!DOCTYPE html>
  <html>
  <head>
      <title>{% block title %}MyProject{% endblock %}</title>
  </head>
  <body>
      <header>
          <h1>MyProject</h1>
      </header>
      <main>
          {% block content %}{% endblock %}
      </main>
      <footer>
          <p>&copy; 2024 MyProject</p>
      </footer>
  </body>
  </html>
  ```

- Update `home.html` to extend `base.html`:
  ```html
  {% extends 'base.html' %}

  {% block title %}Home - MyProject{% endblock %}

  {% block content %}
      <h1>{{ message }}</h1>
  {% endblock %}
  ```

### 10. Debugging Tips

- Ensure that `DIRS` in `settings.py` points to the correct template directory.
- Check for typos in template names or paths.
- Use the `{% debug %}` template tag to inspect the context variables passed to templates.

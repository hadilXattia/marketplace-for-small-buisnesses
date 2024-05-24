python -m venv env

source env/bin/activate

pip install django

django-admin startproject kaizen

 python [manage.py](http://manage.py) startapp nom_app

to run ther server : python [manage.py](http://manage.py) runserver

role of each file : 
manage.py : man file for creating super user and other like develeppment server

[asgi.py](http://asgi.py) and [wsgi.py](http://wsgi.py) are the entery point for the server used in deployment part

[settings.py](http://settings.py) : global config for the project has in it the secret key installed apps , permessions to data base and structure of password

[urls.py](http://urls.py) : 

[apps.py](http://apps.py) : config file just for the specified app (dossier mere )

[models.py](http://models.py) : defining models 

---

---

syntax new staff

This code defines two Django form classes: **`LoginForm`** and **`SignupForm`**

### **Imports**

```python
from django import forms  --rovides a way to create HTML forms
from django.contrib.auth.forms import UserCreationForm, AuthenticationForm --
from django.contrib.auth.models import User --user model provided by Django.

```

### **LoginForm Class**

```python
pythonCopy code
class LoginForm(AuthenticationForm):
    username = forms.CharField(
        widget=forms.TextInput(
            attrs={
                "placeholder": "Your username",
                "class": "w-full py-4 px-6 rounded-xl",
            }
        )
    )
    password = forms.CharField(
        widget=forms.PasswordInput(
            attrs={
                "placeholder": "Your password",
                "class": "w-full py-4 px-6 rounded-xl",
            }
        )
    )

```

- **`placeholder`**: Provides a placeholder text within the input fields.
- **`class`**: Adds CSS classes to style the input fields with specific padding (**`py-4 px-6`**) and rounded corners (**`rounded-xl`**), likely corresponding to a utility-first CSS framework like Tailwind CSS.

---

```python
from django.contrib.auth import views as auth_views
from django.urls import path

from . import views
from .forms import LoginForm

app_name = "core"

urlpatterns = [
    path("", views.index, name="index"),
    path("contact/", views.contact, name="contact"),
    path("signup/", views.signup, name="signup"),
    path(
        "login/",
        auth_views.LoginView.as_view(
            template_name="core/login.html", authentication_form=LoginForm
        ),
        name="login",
    ),
]

```

- **`auth_views`**: Imports the Django built-in authentication views, specifically for handling user login.
- **`path`**: Function used to define URL patterns.
- **`views`**: Imports the views from the current application (assuming the views are defined in the same directory).
- **`LoginForm`**: Imports the custom login form defined earlier.

This code sets up URL routing for a Django application named "core". It includes paths for the index page, contact page, signup page, and login page. The login page uses Django's built-in **`LoginView`**, customized with a specific template and a custom authentication form. Each URL pattern is named for easy reference and URL reversing.

---

---

### template part

This code is a Django template that extends a base template and defines content blocks for a webpage. The template uses HTML, along with Django template tags and filters, to render dynamic content. Here's a breakdown of each part:

### **Extending the Base Template**

```html
{% extends 'core/base.html' %}
```

- This line indicates that this template extends **`core/base.html`**, meaning it inherits the structure and common elements defined in the base template.

### **Block Title**

```html
{% block title %}Welcome{% endblock %}
```

- This block defines the content for the **`title`** block in the base template. The title of this page will be "Welcome".

### **Block Content**

```html
{% block content %}
    <div class="mt-6 px-6 py-12 bg-gray-100 rounded-xl">
        <h2 class="mb-12 text-3xl text-center font-bold text-purple-700">Newest items</h2>

```

- This starts the **`content`** block where the main content of the page is defined.
- The outer **`<div>`** provides padding, margin, background color, and rounded corners for the section displaying the newest items.

### **Displaying Newest Items**

```
   <div class="grid grid-cols-3 gap-3">
            {% for item in items %}
                <div>
                    <a href="{% url 'item:detail' item.id %}">
                        <div>
                            <img src="{{ item.image.url }}" class="rounded-t-xl">
                        </div>
                        <div class="p-6 bg-white rounded-b-xl">
                            <h2 class="text-xl">{{ item.name }}</h2>
                            <p class="text-gray-500">Price: {{ item.price }}</p>
                        </div>
                    </a>
                </div>
            {% endfor %}
        </div>

```

- This section uses a grid layout (**`grid grid-cols-3 gap-3`**) to display items in a 3-column grid with gaps between the columns.
- **`{% for item in items %}`**: Loops through each item in the **`items`** context variable.
- Each item is wrapped in a link (**`<a>`**) that navigates to the item's detail page. The URL is generated using the **`url`** template tag with the item ID.
- The image of the item is displayed inside a **`div`** with the image URL obtained from **`item.image.url`** and styled with rounded top corners.
- The item's name and price are displayed in a nested **`div`** with padding and background color, styled with rounded bottom corners.

### **Displaying Categories**

```

    <div class="mt-6 px-6 py-12 bg-gray-100 rounded-xl">
        <h2 class="mb-12 text-2xl text-center">Categories</h2>
        <div class="grid grid-cols-3 gap-3">
            {% for category in categories %}
                <div>
                    <a href="#">
                        <div class="p-6 bg-white rounded-b-xl">
                            <h2 class="text-2xl">{{ category.name }}</h2>
                            <p class="text-gray-500">{{ category.items.count }} items</p>
                        </div>
                    </a>
                </div>
            {% endfor %}
        </div>
    </div>
{% endblock %}

```

- This section similarly uses a grid layout to display categories.
- **`{% for category in categories %}`**: Loops through each category in the **`categories`** context variable.
- Each category is wrapped in a link (**`<a>`**), though the **`href`** attribute is set to **`"#"`** as a placeholder.
- The category name and the count of items in the category are displayed in a **`div`** with padding and background color, styled with rounded bottom corners.

### **Summary**

- The template extends a base template and defines a **`title`** block and a **`content`** block.
- The **`content`** block contains two main sections: one for displaying the newest items and one for displaying categories.
- It uses Django template tags to loop through **`items`** and **`categories`**, generating HTML for each item and category dynamically.
- The template uses Tailwind CSS classes for styling, providing a modern and responsive layout.

# Django — Beginner to Advanced

> Complete Django learning guide from **Beginner → Intermediate → Advanced** with full code examples and **English 🇬🇧 + Khmer 🇰🇭** explanations.

[![Django](https://img.shields.io/badge/Django-6.0-092E20?logo=django\&logoColor=white)](https://www.djangoproject.com/)
[![Python](https://img.shields.io/badge/Python-3.12%2B-3776AB?logo=python\&logoColor=white)](https://www.python.org/)
[![License](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)

---

## 📚 Table of Contents

* [1. What is Django?](#1-what-is-django)
* [2. Django Architecture](#2-django-architecture)
* [3. Requirements](#3-requirements)
* [4. Installation](#4-installation)
* [5. Create Your First Project](#5-create-your-first-project)
* [6. Create Your First App](#6-create-your-first-app)
* [7. Project Structure](#7-project-structure)
* [8. Settings](#8-settings)
* [9. URLs](#9-urls)
* [10. Views](#10-views)
* [11. Templates](#11-templates)
* [12. Static Files](#12-static-files)
* [13. Models](#13-models)
* [14. Migrations](#14-migrations)
* [15. Django ORM](#15-django-orm)
* [16. CRUD](#16-crud)
* [17. Django Admin](#17-django-admin)
* [18. Forms](#18-forms)
* [19. ModelForms](#19-modelforms)
* [20. Validation](#20-validation)
* [21. Authentication](#21-authentication)
* [22. Authorization](#22-authorization)
* [23. Sessions](#23-sessions)
* [24. Messages](#24-messages)
* [25. Class-Based Views](#25-class-based-views)
* [26. Generic Views](#26-generic-views)
* [27. Relationships](#27-relationships)
* [28. File Uploads](#28-file-uploads)
* [29. Pagination](#29-pagination)
* [30. Search](#30-search)
* [31. Transactions](#31-transactions)
* [32. Custom Management Commands](#32-custom-management-commands)
* [33. Middleware](#33-middleware)
* [34. Signals](#34-signals)
* [35. Custom User Model](#35-custom-user-model)
* [36. Email](#36-email)
* [37. JSON API](#37-json-api)
* [38. Async Views](#38-async-views)
* [39. Caching](#39-caching)
* [40. Testing](#40-testing)
* [41. Security](#41-security)
* [42. Environment Variables](#42-environment-variables)
* [43. PostgreSQL](#43-postgresql)
* [44. Production Settings](#44-production-settings)
* [45. Static Files in Production](#45-static-files-in-production)
* [46. Deployment](#46-deployment)
* [47. Performance Optimization](#47-performance-optimization)
* [48. Large Project Structure](#48-large-project-structure)
* [49. Useful Commands](#49-useful-commands)
* [50. Complete Mini Project](#50-complete-mini-project)
* [51. GitHub Workflow](#51-github-workflow)
* [52. Learning Roadmap](#52-learning-roadmap)
* [53. Official Documentation](#53-official-documentation)

---

# 1. What is Django?

## 🇬🇧 English

[Django](https://www.djangoproject.com/) is a high-level Python web framework for building secure, scalable, and maintainable web applications.

Django provides many features out of the box:

* URL routing
* Views
* Templates
* ORM
* Database migrations
* Authentication
* Authorization
* Admin panel
* Forms
* Sessions
* Security
* Middleware
* Testing
* Caching
* Email
* File uploads
* Async support

## 🇰🇭 Khmer

Django គឺជា **Web Framework សម្រាប់ Python** ដែលប្រើសម្រាប់បង្កើត Web Application។

Django មានមុខងារសំខាន់ៗជាច្រើនស្រាប់៖

* URL Routing
* Views
* Templates
* Database ORM
* Migration
* Login / Logout
* Authentication
* Authorization
* Admin Panel
* Forms
* Sessions
* Security
* Middleware
* Testing
* Caching
* Email
* File Upload
* Async

---

# 2. Django Architecture

Django commonly uses the **MTV architecture**.

```text
                    ┌──────────────┐
                    │    Browser   │
                    └──────┬───────┘
                           │
                           ▼
                    ┌──────────────┐
                    │     URL      │
                    └──────┬───────┘
                           │
                           ▼
                    ┌──────────────┐
                    │     View     │
                    └───┬──────┬───┘
                        │      │
                        │      ▼
                        │  ┌──────────┐
                        │  │  Model   │
                        │  └────┬─────┘
                        │       │
                        │       ▼
                        │  ┌──────────┐
                        │  │ Database │
                        │  └──────────┘
                        │
                        ▼
                    ┌──────────────┐
                    │   Template   │
                    └──────┬───────┘
                           │
                           ▼
                    ┌──────────────┐
                    │    HTML      │
                    └──────────────┘
```

### Model

Handles database data.

```python
from django.db import models


class Product(models.Model):
    name = models.CharField(max_length=100)
    price = models.DecimalField(
        max_digits=10,
        decimal_places=2,
    )
```

### View

Handles application logic.

```python
from django.shortcuts import render

from .models import Product


def product_list(request):
    products = Product.objects.all()

    return render(
        request,
        "products/list.html",
        {"products": products},
    )
```

### Template

Displays data.

```html
<h1>{{ product.name }}</h1>

<p>${{ product.price }}</p>
```

### 🇰🇭 Khmer

* **Model** → គ្រប់គ្រង Database
* **View** → គ្រប់គ្រង Application Logic
* **Template** → បង្ហាញ HTML
* **URL** → ភ្ជាប់ URL ទៅ View

---

# 3. Requirements

Django 6.0 supports:

```text
Python 3.12
Python 3.13
Python 3.14
```

Official compatibility information:

* [Django 6.0 Installation](https://docs.djangoproject.com/en/6.0/intro/install/)
* [Django Python Compatibility](https://docs.djangoproject.com/en/6.0/faq/install/)

Recommended tools:

```text
Python 3.12+
Django 6.0
Git
VS Code
PostgreSQL
```

Check Python:

```bash
python --version
```

Check Git:

```bash
git --version
```

---

# 4. Installation

## Windows

Create project folder:

```powershell
mkdir django-project
cd django-project
```

Create virtual environment:

```powershell
python -m venv venv
```

Activate:

```powershell
venv\Scripts\activate
```

Upgrade pip:

```powershell
python -m pip install --upgrade pip
```

Install Django:

```powershell
python -m pip install "Django>=6.0,<6.1"
```

Check version:

```powershell
python -m django --version
```

Expected:

```text
6.0.x
```

---

## Linux / macOS

```bash
mkdir django-project
cd django-project

python3 -m venv venv

source venv/bin/activate

python -m pip install --upgrade pip

python -m pip install "Django>=6.0,<6.1"

python -m django --version
```

---

# 5. Create Your First Project

Create:

```bash
django-admin startproject config .
```

Run server:

```bash
python manage.py runserver
```

Open:

```text
http://127.0.0.1:8000/
```

Official tutorial:

[Writing your first Django app](https://docs.djangoproject.com/en/6.0/intro/tutorial01/)

---

# 6. Create Your First App

Create app:

```bash
python manage.py startapp products
```

Add it to:

```python
# config/settings.py

INSTALLED_APPS = [
    "django.contrib.admin",
    "django.contrib.auth",
    "django.contrib.contenttypes",
    "django.contrib.sessions",
    "django.contrib.messages",
    "django.contrib.staticfiles",

    "products",
]
```

---

# 7. Project Structure

After setup:

```text
django-project/
│
├── manage.py
│
├── config/
│   ├── __init__.py
│   ├── settings.py
│   ├── urls.py
│   ├── asgi.py
│   └── wsgi.py
│
├── products/
│   ├── __init__.py
│   ├── admin.py
│   ├── apps.py
│   ├── models.py
│   ├── tests.py
│   ├── views.py
│   └── migrations/
│       └── __init__.py
│
├── templates/
│
├── static/
│
├── media/
│
├── requirements.txt
│
├── .env
│
└── .gitignore
```

---

# 8. Settings

```python
# config/settings.py

from pathlib import Path


BASE_DIR = Path(__file__).resolve().parent.parent


SECRET_KEY = "development-only-change-in-production"


DEBUG = True


ALLOWED_HOSTS = []


INSTALLED_APPS = [
    "django.contrib.admin",
    "django.contrib.auth",
    "django.contrib.contenttypes",
    "django.contrib.sessions",
    "django.contrib.messages",
    "django.contrib.staticfiles",

    "products",
]


MIDDLEWARE = [
    "django.middleware.security.SecurityMiddleware",

    "django.contrib.sessions.middleware.SessionMiddleware",

    "django.middleware.common.CommonMiddleware",

    "django.middleware.csrf.CsrfViewMiddleware",

    "django.contrib.auth.middleware.AuthenticationMiddleware",

    "django.contrib.messages.middleware.MessageMiddleware",

    "django.middleware.clickjacking.XFrameOptionsMiddleware",
]


ROOT_URLCONF = "config.urls"


TEMPLATES = [
    {
        "BACKEND": "django.template.backends.django.DjangoTemplates",

        "DIRS": [
            BASE_DIR / "templates",
        ],

        "APP_DIRS": True,

        "OPTIONS": {
            "context_processors": [
                "django.template.context_processors.request",

                "django.contrib.auth.context_processors.auth",

                "django.contrib.messages.context_processors.messages",
            ],
        },
    },
]


WSGI_APPLICATION = "config.wsgi.application"

ASGI_APPLICATION = "config.asgi.application"


DATABASES = {
    "default": {
        "ENGINE": "django.db.backends.sqlite3",
        "NAME": BASE_DIR / "db.sqlite3",
    }
}


LANGUAGE_CODE = "en-us"


TIME_ZONE = "Asia/Phnom_Penh"


USE_I18N = True

USE_TZ = True


STATIC_URL = "static/"

STATICFILES_DIRS = [
    BASE_DIR / "static",
]


MEDIA_URL = "media/"

MEDIA_ROOT = BASE_DIR / "media"


DEFAULT_AUTO_FIELD = "django.db.models.BigAutoField"


LOGIN_REDIRECT_URL = "/"

LOGOUT_REDIRECT_URL = "/"
```

---

# 9. URLs

## Main URL

```python
# config/urls.py

from django.contrib import admin
from django.urls import include, path


urlpatterns = [
    path(
        "admin/",
        admin.site.urls,
    ),

    path(
        "products/",
        include("products.urls"),
    ),
]
```

## App URL

Create:

```text
products/urls.py
```

```python
# products/urls.py

from django.urls import path

from . import views


urlpatterns = [
    path(
        "",
        views.product_list,
        name="product-list",
    ),

    path(
        "<int:product_id>/",
        views.product_detail,
        name="product-detail",
    ),
]
```

---

# 10. Views

```python
# products/views.py

from django.shortcuts import get_object_or_404
from django.shortcuts import render

from .models import Product


def product_list(request):
    products = Product.objects.all()

    return render(
        request,
        "products/list.html",
        {
            "products": products,
        },
    )


def product_detail(request, product_id):
    product = get_object_or_404(
        Product,
        id=product_id,
    )

    return render(
        request,
        "products/detail.html",
        {
            "product": product,
        },
    )
```

---

# 11. Templates

Create:

```text
templates/
└── products/
    ├── list.html
    └── detail.html
```

## list.html

```html
<!DOCTYPE html>

<html lang="en">

<head>
    <meta charset="UTF-8">

    <meta
        name="viewport"
        content="width=device-width, initial-scale=1.0"
    >

    <title>Products</title>
</head>

<body>

    <h1>Products</h1>

    {% for product in products %}

        <article>

            <h2>
                <a
                    href="{% url 'product-detail' product.id %}"
                >
                    {{ product.name }}
                </a>
            </h2>

            <p>
                Price: ${{ product.price }}
            </p>

        </article>

    {% empty %}

        <p>
            No products found.
        </p>

    {% endfor %}

</body>

</html>
```

## detail.html

```html
<!DOCTYPE html>

<html lang="en">

<head>
    <meta charset="UTF-8">

    <title>{{ product.name }}</title>
</head>

<body>

    <h1>
        {{ product.name }}
    </h1>

    <p>
        Price: ${{ product.price }}
    </p>

    <a href="{% url 'product-list' %}">
        Back to products
    </a>

</body>

</html>
```

---

# 12. Static Files

Create:

```text
static/
└── css/
    └── style.css
```

```css
/* static/css/style.css */

body {
    font-family: Arial, sans-serif;
    max-width: 1000px;
    margin: 0 auto;
    padding: 30px;
}

h1 {
    margin-bottom: 30px;
}

article {
    border: 1px solid #ddd;
    padding: 20px;
    margin-bottom: 15px;
}
```

Load CSS:

```html
{% load static %}

<!DOCTYPE html>

<html lang="en">

<head>

    <meta charset="UTF-8">

    <link
        rel="stylesheet"
        href="{% static 'css/style.css' %}"
    >

    <title>Products</title>

</head>

<body>

    <h1>Products</h1>

</body>

</html>
```

Official guide:

[Managing static files](https://docs.djangoproject.com/en/6.0/howto/static-files/)

---

# 13. Models

```python
# products/models.py

from django.db import models


class Product(models.Model):

    name = models.CharField(
        max_length=100,
    )

    description = models.TextField(
        blank=True,
    )

    price = models.DecimalField(
        max_digits=10,
        decimal_places=2,
    )

    quantity = models.PositiveIntegerField(
        default=0,
    )

    created_at = models.DateTimeField(
        auto_now_add=True,
    )

    updated_at = models.DateTimeField(
        auto_now=True,
    )


    def __str__(self):
        return self.name
```

Official documentation:

[Models](https://docs.djangoproject.com/en/6.0/topics/db/models/)

[Model API Reference](https://docs.djangoproject.com/en/6.0/ref/models/)

---

# 14. Migrations

Create migrations:

```bash
python manage.py makemigrations
```

Apply migrations:

```bash
python manage.py migrate
```

Check migrations:

```bash
python manage.py showmigrations
```

Migration flow:

```text
models.py
    ↓
makemigrations
    ↓
migration files
    ↓
migrate
    ↓
Database
```

Official documentation:

[Migrations](https://docs.djangoproject.com/en/6.0/topics/migrations/)

---

# 15. Django ORM

## Create

```python
product = Product.objects.create(
    name="Laptop",
    description="Development laptop",
    price=999.99,
    quantity=10,
)
```

## Get all

```python
products = Product.objects.all()
```

## Get one

```python
product = Product.objects.get(id=1)
```

## Filter

```python
products = Product.objects.filter(
    price__gte=500,
)
```

## Exclude

```python
products = Product.objects.exclude(
    quantity=0,
)
```

## Order

```python
products = Product.objects.order_by(
    "-price",
)
```

## Count

```python
total = Product.objects.count()
```

## Update

```python
Product.objects.filter(
    id=1,
).update(
    price=1200,
)
```

## Delete

```python
Product.objects.filter(
    id=1,
).delete()
```

Official documentation:

[QuerySets](https://docs.djangoproject.com/en/6.0/topics/db/queries/)

[QuerySet API](https://docs.djangoproject.com/en/6.0/ref/models/querysets/)

---

# 16. CRUD

CRUD means:

```text
C = Create
R = Read
U = Update
D = Delete
```

## Create

```python
Product.objects.create(
    name="Keyboard",
    price=50,
    quantity=20,
)
```

## Read

```python
products = Product.objects.all()
```

## Update

```python
product = Product.objects.get(
    id=1,
)

product.price = 60

product.save()
```

## Delete

```python
product = Product.objects.get(
    id=1,
)

product.delete()
```

---

# 17. Django Admin

Create superuser:

```bash
python manage.py createsuperuser
```

Register model:

```python
# products/admin.py

from django.contrib import admin

from .models import Product


@admin.register(Product)
class ProductAdmin(admin.ModelAdmin):

    list_display = (
        "id",
        "name",
        "price",
        "quantity",
        "created_at",
    )

    search_fields = (
        "name",
        "description",
    )

    list_filter = (
        "created_at",
    )
```

Run:

```bash
python manage.py runserver
```

Open:

```text
http://127.0.0.1:8000/admin/
```

Official documentation:

[Django Admin](https://docs.djangoproject.com/en/6.0/ref/contrib/admin/)

---

# 18. Forms

```python
# products/forms.py

from django import forms


class ProductForm(forms.Form):

    name = forms.CharField(
        max_length=100,
    )

    price = forms.DecimalField(
        max_digits=10,
        decimal_places=2,
    )

    quantity = forms.IntegerField(
        min_value=0,
    )
```

View:

```python
from django.shortcuts import redirect
from django.shortcuts import render

from .forms import ProductForm
from .models import Product


def product_create(request):

    if request.method == "POST":

        form = ProductForm(
            request.POST,
        )

        if form.is_valid():

            Product.objects.create(
                name=form.cleaned_data["name"],
                price=form.cleaned_data["price"],
                quantity=form.cleaned_data["quantity"],
            )

            return redirect(
                "product-list",
            )

    else:

        form = ProductForm()

    return render(
        request,
        "products/create.html",
        {
            "form": form,
        },
    )
```

Template:

```html
<h1>Create Product</h1>

<form method="post">

    {% csrf_token %}

    {{ form.as_p }}

    <button type="submit">
        Create
    </button>

</form>
```

Official documentation:

[Forms](https://docs.djangoproject.com/en/6.0/topics/forms/)

---

# 19. ModelForms

```python
# products/forms.py

from django import forms

from .models import Product


class ProductForm(forms.ModelForm):

    class Meta:
        model = Product

        fields = [
            "name",
            "description",
            "price",
            "quantity",
        ]
```

View:

```python
from django.shortcuts import redirect
from django.shortcuts import render

from .forms import ProductForm


def product_create(request):

    if request.method == "POST":

        form = ProductForm(
            request.POST,
        )

        if form.is_valid():

            form.save()

            return redirect(
                "product-list",
            )

    else:

        form = ProductForm()

    return render(
        request,
        "products/create.html",
        {
            "form": form,
        },
    )
```

---

# 20. Validation

```python
from django import forms

from .models import Product


class ProductForm(forms.ModelForm):

    class Meta:
        model = Product

        fields = [
            "name",
            "description",
            "price",
            "quantity",
        ]


    def clean_price(self):

        price = self.cleaned_data["price"]

        if price < 0:

            raise forms.ValidationError(
                "Price cannot be negative.",
            )

        return price
```

Official documentation:

[Form validation](https://docs.djangoproject.com/en/6.0/ref/forms/validation/)

---

# 21. Authentication

Django includes authentication functionality.

URLs:

```python
# config/urls.py

from django.contrib import admin
from django.contrib.auth import views as auth_views
from django.urls import include
from django.urls import path


urlpatterns = [
    path(
        "admin/",
        admin.site.urls,
    ),

    path(
        "products/",
        include("products.urls"),
    ),

    path(
        "login/",
        auth_views.LoginView.as_view(
            template_name="registration/login.html",
        ),
        name="login",
    ),

    path(
        "logout/",
        auth_views.LogoutView.as_view(),
        name="logout",
    ),
]
```

Login template:

```html
<!-- templates/registration/login.html -->

<h1>Login</h1>

<form method="post">

    {% csrf_token %}

    {{ form.as_p }}

    <button type="submit">
        Login
    </button>

</form>
```

Settings:

```python
LOGIN_REDIRECT_URL = "/"

LOGOUT_REDIRECT_URL = "/"
```

Official documentation:

[Authentication](https://docs.djangoproject.com/en/6.0/topics/auth/)

---

# 22. Authorization

Function-based view:

```python
from django.contrib.auth.decorators import login_required
from django.shortcuts import render


@login_required
def dashboard(request):

    return render(
        request,
        "dashboard.html",
    )
```

Class-based view:

```python
from django.contrib.auth.mixins import LoginRequiredMixin
from django.views.generic import TemplateView


class DashboardView(
    LoginRequiredMixin,
    TemplateView,
):

    template_name = "dashboard.html"
```

---

# 23. Sessions

Save:

```python
request.session["cart"] = [
    1,
    2,
    3,
]
```

Read:

```python
cart = request.session.get(
    "cart",
    [],
)
```

Delete:

```python
request.session.pop(
    "cart",
    None,
)
```

Clear:

```python
request.session.flush()
```

Official documentation:

[Sessions](https://docs.djangoproject.com/en/6.0/topics/http/sessions/)

---

# 24. Messages

```python
from django.contrib import messages
from django.shortcuts import redirect


def create_product(request):

    # Create product here

    messages.success(
        request,
        "Product created successfully.",
    )

    return redirect(
        "product-list",
    )
```

Template:

```html
{% if messages %}

    {% for message in messages %}

        <div>
            {{ message }}
        </div>

    {% endfor %}

{% endif %}
```

Types:

```python
messages.debug()
messages.info()
messages.success()
messages.warning()
messages.error()
```

Official documentation:

[Messages Framework](https://docs.djangoproject.com/en/6.0/ref/contrib/messages/)

---

# 25. Class-Based Views

```python
from django.views import View
from django.shortcuts import render

from .models import Product


class ProductListView(View):

    def get(self, request):

        products = Product.objects.all()

        return render(
            request,
            "products/list.html",
            {
                "products": products,
            },
        )
```

URL:

```python
from django.urls import path

from .views import ProductListView


urlpatterns = [
    path(
        "",
        ProductListView.as_view(),
        name="product-list",
    ),
]
```

---

# 26. Generic Views

## ListView

```python
from django.views.generic import ListView

from .models import Product


class ProductListView(ListView):

    model = Product

    template_name = "products/list.html"

    context_object_name = "products"
```

## DetailView

```python
from django.views.generic import DetailView

from .models import Product


class ProductDetailView(DetailView):

    model = Product

    template_name = "products/detail.html"

    context_object_name = "product"
```

## CreateView

```python
from django.urls import reverse_lazy
from django.views.generic import CreateView

from .models import Product


class ProductCreateView(CreateView):

    model = Product

    fields = [
        "name",
        "description",
        "price",
        "quantity",
    ]

    template_name = "products/create.html"

    success_url = reverse_lazy(
        "product-list",
    )
```

## UpdateView

```python
from django.urls import reverse_lazy
from django.views.generic import UpdateView

from .models import Product


class ProductUpdateView(UpdateView):

    model = Product

    fields = [
        "name",
        "description",
        "price",
        "quantity",
    ]

    template_name = "products/update.html"

    success_url = reverse_lazy(
        "product-list",
    )
```

## DeleteView

```python
from django.urls import reverse_lazy
from django.views.generic import DeleteView

from .models import Product


class ProductDeleteView(DeleteView):

    model = Product

    template_name = "products/delete.html"

    success_url = reverse_lazy(
        "product-list",
    )
```

Official documentation:

[Generic Views](https://docs.djangoproject.com/en/6.0/topics/class-based-views/generic-display/)

[Generic Editing Views](https://docs.djangoproject.com/en/6.0/topics/class-based-views/generic-editing/)

---

# 27. Relationships

## ForeignKey

```python
class Category(models.Model):

    name = models.CharField(
        max_length=100,
    )


class Product(models.Model):

    category = models.ForeignKey(
        Category,
        on_delete=models.CASCADE,
        related_name="products",
    )

    name = models.CharField(
        max_length=100,
    )

    price = models.DecimalField(
        max_digits=10,
        decimal_places=2,
    )
```

Query:

```python
category = Category.objects.get(
    id=1,
)

products = category.products.all()
```

---

## OneToOneField

```python
class UserProfile(models.Model):

    user = models.OneToOneField(
        "auth.User",
        on_delete=models.CASCADE,
    )

    phone = models.CharField(
        max_length=30,
        blank=True,
    )
```

---

## ManyToManyField

```python
class Tag(models.Model):

    name = models.CharField(
        max_length=100,
    )


class Product(models.Model):

    name = models.CharField(
        max_length=100,
    )

    tags = models.ManyToManyField(
        Tag,
        blank=True,
    )
```

Official documentation:

[Relationships](https://docs.djangoproject.com/en/6.0/topics/db/examples/)

---

# 28. File Uploads

Install Pillow:

```bash
python -m pip install pillow
```

Model:

```python
class Product(models.Model):

    name = models.CharField(
        max_length=100,
    )

    image = models.ImageField(
        upload_to="products/",
        blank=True,
        null=True,
    )
```

Settings:

```python
MEDIA_URL = "/media/"

MEDIA_ROOT = BASE_DIR / "media"
```

URLs:

```python
from django.conf import settings
from django.conf.urls.static import static
from django.contrib import admin
from django.urls import path


urlpatterns = [
    path(
        "admin/",
        admin.site.urls,
    ),
]

urlpatterns += static(
    settings.MEDIA_URL,
    document_root=settings.MEDIA_ROOT,
)
```

Form:

```html
<form
    method="post"
    enctype="multipart/form-data"
>

    {% csrf_token %}

    {{ form.as_p }}

    <button type="submit">
        Upload
    </button>

</form>
```

View:

```python
form = ProductForm(
    request.POST,
    request.FILES,
)
```

Official documentation:

[File Uploads](https://docs.djangoproject.com/en/6.0/topics/http/file-uploads/)

---

# 29. Pagination

```python
from django.core.paginator import Paginator
from django.shortcuts import render

from .models import Product


def product_list(request):

    products = Product.objects.all()

    paginator = Paginator(
        products,
        10,
    )

    page_number = request.GET.get(
        "page",
    )

    page_obj = paginator.get_page(
        page_number,
    )

    return render(
        request,
        "products/list.html",
        {
            "page_obj": page_obj,
        },
    )
```

Template:

```html
{% for product in page_obj %}

    <h2>
        {{ product.name }}
    </h2>

{% endfor %}


{% if page_obj.has_previous %}

    <a
        href="?page={{ page_obj.previous_page_number }}"
    >
        Previous
    </a>

{% endif %}


<span>
    Page {{ page_obj.number }}
    of
    {{ page_obj.paginator.num_pages }}
</span>


{% if page_obj.has_next %}

    <a
        href="?page={{ page_obj.next_page_number }}"
    >
        Next
    </a>

{% endif %}
```

Official documentation:

[Pagination](https://docs.djangoproject.com/en/6.0/topics/pagination/)

---

# 30. Search

```python
from django.db.models import Q
from django.shortcuts import render

from .models import Product


def product_list(request):

    query = request.GET.get(
        "q",
        "",
    )

    products = Product.objects.all()

    if query:

        products = products.filter(
            Q(name__icontains=query)
            |
            Q(description__icontains=query)
        )

    return render(
        request,
        "products/list.html",
        {
            "products": products,
            "query": query,
        },
    )
```

Template:

```html
<form method="get">

    <input
        type="search"
        name="q"
        value="{{ query }}"
        placeholder="Search products..."
    >

    <button type="submit">
        Search
    </button>

</form>
```

---

# 31. Transactions

Use transactions when multiple database operations should succeed or fail together.

```python
from django.db import transaction


def create_order():

    with transaction.atomic():

        order = Order.objects.create(
            customer="Heng",
        )

        OrderItem.objects.create(
            order=order,
            product_id=1,
            quantity=2,
        )

        return order
```

If an exception occurs inside the transaction, Django rolls back the transaction.

Official documentation:

[Database Transactions](https://docs.djangoproject.com/en/6.0/topics/db/transactions/)

---

# 32. Custom Management Commands

Structure:

```text
products/
└── management/
    ├── __init__.py
    └── commands/
        ├── __init__.py
        └── seed_products.py
```

Code:

```python
from django.core.management.base import BaseCommand

from products.models import Product


class Command(BaseCommand):

    help = "Create sample products"


    def handle(
        self,
        *args,
        **options,
    ):

        Product.objects.create(
            name="Laptop",
            price=999,
            quantity=10,
        )

        Product.objects.create(
            name="Keyboard",
            price=50,
            quantity=20,
        )

        self.stdout.write(
            self.style.SUCCESS(
                "Products created successfully.",
            ),
        )
```

Run:

```bash
python manage.py seed_products
```

Official documentation:

[Custom Management Commands](https://docs.djangoproject.com/en/6.0/howto/custom-management-commands/)

---

# 33. Middleware

```python
# products/middleware.py

import time


class RequestTimeMiddleware:

    def __init__(self, get_response):

        self.get_response = get_response


    def __call__(self, request):

        start = time.perf_counter()

        response = self.get_response(
            request,
        )

        duration = (
            time.perf_counter() - start
        )

        print(
            f"Request took {duration:.4f}s",
        )

        return response
```

Add:

```python
MIDDLEWARE = [
    "django.middleware.security.SecurityMiddleware",

    "django.contrib.sessions.middleware.SessionMiddleware",

    "django.middleware.common.CommonMiddleware",

    "django.middleware.csrf.CsrfViewMiddleware",

    "django.contrib.auth.middleware.AuthenticationMiddleware",

    "django.contrib.messages.middleware.MessageMiddleware",

    "django.middleware.clickjacking.XFrameOptionsMiddleware",

    "products.middleware.RequestTimeMiddleware",
]
```

Official documentation:

[Middleware](https://docs.djangoproject.com/en/6.0/topics/http/middleware/)

---

# 34. Signals

```python
# products/signals.py

from django.db.models.signals import post_save
from django.dispatch import receiver

from .models import Product


@receiver(
    post_save,
    sender=Product,
)
def product_created(
    sender,
    instance,
    created,
    **kwargs,
):

    if created:

        print(
            f"Created product: {instance.name}",
        )
```

`apps.py`:

```python
from django.apps import AppConfig


class ProductsConfig(AppConfig):

    default_auto_field = (
        "django.db.models.BigAutoField"
    )

    name = "products"


    def ready(self):

        from . import signals
```

Official documentation:

[Signals](https://docs.djangoproject.com/en/6.0/topics/signals/)

---

# 35. Custom User Model

For a new project, decide early whether you need a custom user model.

Create:

```python
# accounts/models.py

from django.contrib.auth.models import AbstractUser
from django.db import models


class User(AbstractUser):

    phone = models.CharField(
        max_length=30,
        blank=True,
    )
```

Settings:

```python
AUTH_USER_MODEL = "accounts.User"
```

Use:

```python
from django.contrib.auth import get_user_model


User = get_user_model()
```

Official documentation:

[Customizing Authentication](https://docs.djangoproject.com/en/6.0/topics/auth/customizing/)

---

# 36. Email

Development email backend:

```python
EMAIL_BACKEND = (
    "django.core.mail.backends.console.EmailBackend"
)
```

Send email:

```python
from django.core.mail import send_mail


send_mail(
    subject="Welcome",
    message="Welcome to our website.",
    from_email="noreply@example.com",
    recipient_list=[
        "user@example.com",
    ],
)
```

Official documentation:

[Sending Email](https://docs.djangoproject.com/en/6.0/topics/email/)

---

# 37. JSON API

Django can return JSON directly.

```python
from django.http import JsonResponse

from .models import Product


def product_api(request):

    products = Product.objects.all()

    data = [
        {
            "id": product.id,
            "name": product.name,
            "price": str(product.price),
        }
        for product in products
    ]

    return JsonResponse(
        {
            "products": data,
        },
    )
```

URL:

```python
path(
    "api/products/",
    views.product_api,
    name="product-api",
)
```

Response:

```json
{
    "products": [
        {
            "id": 1,
            "name": "Laptop",
            "price": "999.00"
        }
    ]
}
```

For a full REST API, consider:

[Django REST Framework](https://www.django-rest-framework.org/)

---

# 38. Async Views

Django supports asynchronous views.

```python
from django.http import JsonResponse


async def async_products(request):

    return JsonResponse(
        {
            "message": "Hello from async Django",
        },
    )
```

URL:

```python
path(
    "async/",
    views.async_products,
    name="async-products",
)
```

Official documentation:

[Asynchronous Support](https://docs.djangoproject.com/en/6.0/topics/async/)

---

# 39. Caching

Development cache:

```python
CACHES = {
    "default": {
        "BACKEND": (
            "django.core.cache.backends.locmem.LocMemCache"
        ),
        "LOCATION": "django-cache",
    }
}
```

Set:

```python
from django.core.cache import cache


cache.set(
    "product_count",
    100,
    timeout=300,
)
```

Get:

```python
count = cache.get(
    "product_count",
)
```

Delete:

```python
cache.delete(
    "product_count",
)
```

Cache a view:

```python
from django.views.decorators.cache import cache_page


@cache_page(60 * 5)
def product_list(request):

    ...
```

Official documentation:

[Caching](https://docs.djangoproject.com/en/6.0/topics/cache/)

---

# 40. Testing

```python
# products/tests.py

from django.test import TestCase
from django.urls import reverse

from .models import Product


class ProductModelTest(TestCase):

    def test_product_string(self):

        product = Product.objects.create(
            name="Laptop",
            price=999,
            quantity=10,
        )

        self.assertEqual(
            str(product),
            "Laptop",
        )


class ProductViewTest(TestCase):

    def test_product_list(self):

        Product.objects.create(
            name="Laptop",
            price=999,
            quantity=10,
        )

        response = self.client.get(
            reverse(
                "product-list",
            ),
        )

        self.assertEqual(
            response.status_code,
            200,
        )

        self.assertContains(
            response,
            "Laptop",
        )
```

Run:

```bash
python manage.py test
```

Run only products:

```bash
python manage.py test products
```

Official documentation:

[Testing](https://docs.djangoproject.com/en/6.0/topics/testing/)

---

# 41. Security

Django provides protection for many common web security problems, including:

* CSRF
* XSS
* SQL injection
* Clickjacking
* Host header validation
* Secure cookies
* HTTPS configuration

Official documentation:

[Django Security](https://docs.djangoproject.com/en/6.0/topics/security/)

## CSRF

Always use:

```html
<form method="post">

    {% csrf_token %}

    ...

</form>
```

## Production DEBUG

Do not use:

```python
DEBUG = True
```

Use:

```python
DEBUG = False
```

## ALLOWED_HOSTS

```python
ALLOWED_HOSTS = [
    "example.com",
    "www.example.com",
]
```

## HTTPS

Production:

```python
SECURE_SSL_REDIRECT = True

SESSION_COOKIE_SECURE = True

CSRF_COOKIE_SECURE = True
```

## HSTS

```python
SECURE_HSTS_SECONDS = 31536000

SECURE_HSTS_INCLUDE_SUBDOMAINS = True

SECURE_HSTS_PRELOAD = True
```

Run:

```bash
python manage.py check --deploy
```

Django's official security documentation specifically recommends HTTPS, secure cookies, HSTS, proper `ALLOWED_HOSTS`, and keeping `SECRET_KEY` secret. [Security in Django](https://docs.djangoproject.com/en/6.0/topics/security/)

---

# 42. Environment Variables

Install:

```bash
python -m pip install python-dotenv
```

Create:

```text
.env
```

```env
DJANGO_SECRET_KEY=change-this-secret
DJANGO_DEBUG=True
DATABASE_NAME=db.sqlite3
```

Settings:

```python
import os

from dotenv import load_dotenv


load_dotenv()


SECRET_KEY = os.environ[
    "DJANGO_SECRET_KEY"
]


DEBUG = (
    os.environ.get(
        "DJANGO_DEBUG",
        "False",
    ).lower()
    == "true"
)
```

`.gitignore`:

```gitignore
.env
venv/
.venv/
__pycache__/
*.pyc
db.sqlite3
media/
staticfiles/
```

---

# 43. PostgreSQL

Install PostgreSQL driver:

```bash
python -m pip install "psycopg[binary]"
```

Settings:

```python
DATABASES = {
    "default": {
        "ENGINE": "django.db.backends.postgresql",

        "NAME": "mydatabase",

        "USER": "postgres",

        "PASSWORD": "your-password",

        "HOST": "localhost",

        "PORT": "5432",
    }
}
```

Then:

```bash
python manage.py migrate
```

Official documentation:

[PostgreSQL Notes](https://docs.djangoproject.com/en/6.0/ref/databases/#postgresql-notes)

Django officially supports PostgreSQL, MariaDB, MySQL, SQLite, and Oracle.

---

# 44. Production Settings

Example:

```python
DEBUG = False


ALLOWED_HOSTS = [
    "example.com",
    "www.example.com",
]


CSRF_TRUSTED_ORIGINS = [
    "https://example.com",
    "https://www.example.com",
]


SECURE_SSL_REDIRECT = True


SESSION_COOKIE_SECURE = True


CSRF_COOKIE_SECURE = True


SECURE_HSTS_SECONDS = 31536000


SECURE_HSTS_INCLUDE_SUBDOMAINS = True


SECURE_HSTS_PRELOAD = True


X_FRAME_OPTIONS = "DENY"
```

Check:

```bash
python manage.py check --deploy
```

Official documentation:

[Deployment Checklist](https://docs.djangoproject.com/en/6.0/howto/deployment/checklist/)

---

# 45. Static Files in Production

Development:

```python
STATIC_URL = "/static/"
```

Production:

```python
STATIC_URL = "/static/"

STATIC_ROOT = BASE_DIR / "staticfiles"
```

Collect:

```bash
python manage.py collectstatic
```

Official documentation:

[Deploying Static Files](https://docs.djangoproject.com/en/6.0/howto/static-files/deployment/)

---

# 46. Deployment

Typical architecture:

```text
Internet
    |
    v
Nginx
    |
    v
Gunicorn / Uvicorn
    |
    v
Django
    |
    +-------- PostgreSQL
    |
    +-------- Redis
    |
    +-------- Object Storage
```

Install Gunicorn:

```bash
python -m pip install gunicorn
```

Run WSGI:

```bash
gunicorn config.wsgi:application
```

For ASGI:

```bash
gunicorn \
    -k uvicorn.workers.UvicornWorker \
    config.asgi:application
```

Important:

```bash
python manage.py runserver
```

is for development and should not be used as your production server.

Official documentation:

[Deploying Django](https://docs.djangoproject.com/en/6.0/howto/deployment/)

[WSGI Deployment](https://docs.djangoproject.com/en/6.0/howto/deployment/wsgi/)

[ASGI Deployment](https://docs.djangoproject.com/en/6.0/howto/deployment/asgi/)

---

# 47. Performance Optimization

## `select_related()`

Use for:

* ForeignKey
* OneToOneField

```python
products = Product.objects.select_related(
    "category",
)
```

## `prefetch_related()`

Use for:

* ManyToManyField
* Reverse relationships

```python
products = Product.objects.prefetch_related(
    "tags",
)
```

## Bad

```python
products = Product.objects.all()

for product in products:

    print(
        product.category.name,
    )
```

## Better

```python
products = Product.objects.select_related(
    "category",
)

for product in products:

    print(
        product.category.name,
    )
```

## Index

```python
class Product(models.Model):

    name = models.CharField(
        max_length=100,
        db_index=True,
    )
```

Or:

```python
class Product(models.Model):

    name = models.CharField(
        max_length=100,
    )


    class Meta:

        indexes = [
            models.Index(
                fields=["name"],
            ),
        ]
```

Official documentation:

[Optimize Database Access](https://docs.djangoproject.com/en/6.0/topics/db/optimization/)

---

# 48. Large Project Structure

For a large application:

```text
django-shop/
│
├── manage.py
│
├── config/
│   ├── settings/
│   │   ├── __init__.py
│   │   ├── base.py
│   │   ├── development.py
│   │   └── production.py
│   │
│   ├── urls.py
│   ├── asgi.py
│   └── wsgi.py
│
├── apps/
│   ├── accounts/
│   │   ├── migrations/
│   │   ├── admin.py
│   │   ├── apps.py
│   │   ├── forms.py
│   │   ├── models.py
│   │   ├── urls.py
│   │   ├── views.py
│   │   └── tests/
│   │
│   ├── products/
│   │   ├── migrations/
│   │   ├── admin.py
│   │   ├── apps.py
│   │   ├── forms.py
│   │   ├── models.py
│   │   ├── urls.py
│   │   ├── views.py
│   │   └── tests/
│   │
│   ├── orders/
│   ├── payments/
│   └── inventory/
│
├── templates/
├── static/
├── media/
├── requirements.txt
├── .env
└── .gitignore
```

### 🇰🇭 Khmer

Project ធំគួរបែងចែកជា apps:

```text
accounts
products
orders
payments
inventory
reports
```

ដើម្បីឱ្យ project ងាយ:

* Maintain
* Test
* Debug
* Scale
* Collaborate

---

# 49. Useful Commands

## Create project

```bash
django-admin startproject config .
```

## Create app

```bash
python manage.py startapp products
```

## Run server

```bash
python manage.py runserver
```

## Make migrations

```bash
python manage.py makemigrations
```

## Apply migrations

```bash
python manage.py migrate
```

## Create superuser

```bash
python manage.py createsuperuser
```

## Django shell

```bash
python manage.py shell
```

## Test

```bash
python manage.py test
```

## Check

```bash
python manage.py check
```

## Production check

```bash
python manage.py check --deploy
```

## Collect static

```bash
python manage.py collectstatic
```

## Show migrations

```bash
python manage.py showmigrations
```

## Django version

```bash
python -m django --version
```

---

# 50. Complete Mini Project

This example combines:

* Model
* ORM
* CRUD
* Forms
* Templates
* URLs
* Admin
* Search
* Messages
* CSS

## Project Structure

```text
django-shop/
│
├── manage.py
│
├── config/
│   ├── __init__.py
│   ├── settings.py
│   ├── urls.py
│   ├── asgi.py
│   └── wsgi.py
│
├── products/
│   ├── __init__.py
│   ├── admin.py
│   ├── apps.py
│   ├── forms.py
│   ├── models.py
│   ├── urls.py
│   ├── views.py
│   └── migrations/
│
├── templates/
│   ├── base.html
│   └── products/
│       ├── list.html
│       ├── detail.html
│       ├── create.html
│       └── delete.html
│
├── static/
│   └── css/
│       └── style.css
│
├── requirements.txt
└── .gitignore
```

---

## 50.1 Create Project

```bash
mkdir django-shop
cd django-shop

python -m venv venv
```

Windows:

```powershell
venv\Scripts\activate
```

Linux/macOS:

```bash
source venv/bin/activate
```

Install:

```bash
python -m pip install --upgrade pip

python -m pip install "Django>=6.0,<6.1"
```

Create project:

```bash
django-admin startproject config .
```

Create app:

```bash
python manage.py startapp products
```

---

## 50.2 Settings

```python
# config/settings.py

from pathlib import Path


BASE_DIR = Path(__file__).resolve().parent.parent


SECRET_KEY = "development-secret-key"


DEBUG = True


ALLOWED_HOSTS = []


INSTALLED_APPS = [
    "django.contrib.admin",
    "django.contrib.auth",
    "django.contrib.contenttypes",
    "django.contrib.sessions",
    "django.contrib.messages",
    "django.contrib.staticfiles",

    "products",
]


MIDDLEWARE = [
    "django.middleware.security.SecurityMiddleware",

    "django.contrib.sessions.middleware.SessionMiddleware",

    "django.middleware.common.CommonMiddleware",

    "django.middleware.csrf.CsrfViewMiddleware",

    "django.contrib.auth.middleware.AuthenticationMiddleware",

    "django.contrib.messages.middleware.MessageMiddleware",

    "django.middleware.clickjacking.XFrameOptionsMiddleware",
]


ROOT_URLCONF = "config.urls"


TEMPLATES = [
    {
        "BACKEND": "django.template.backends.django.DjangoTemplates",

        "DIRS": [
            BASE_DIR / "templates",
        ],

        "APP_DIRS": True,

        "OPTIONS": {
            "context_processors": [
                "django.template.context_processors.request",

                "django.contrib.auth.context_processors.auth",

                "django.contrib.messages.context_processors.messages",
            ],
        },
    },
]


WSGI_APPLICATION = "config.wsgi.application"

ASGI_APPLICATION = "config.asgi.application"


DATABASES = {
    "default": {
        "ENGINE": "django.db.backends.sqlite3",
        "NAME": BASE_DIR / "db.sqlite3",
    }
}


LANGUAGE_CODE = "en-us"


TIME_ZONE = "Asia/Phnom_Penh"


USE_I18N = True

USE_TZ = True


STATIC_URL = "static/"

STATICFILES_DIRS = [
    BASE_DIR / "static",
]


DEFAULT_AUTO_FIELD = (
    "django.db.models.BigAutoField"
)
```

---

## 50.3 Model

```python
# products/models.py

from django.db import models


class Product(models.Model):

    name = models.CharField(
        max_length=100,
    )

    description = models.TextField(
        blank=True,
    )

    price = models.DecimalField(
        max_digits=10,
        decimal_places=2,
    )

    quantity = models.PositiveIntegerField(
        default=0,
    )

    created_at = models.DateTimeField(
        auto_now_add=True,
    )

    updated_at = models.DateTimeField(
        auto_now=True,
    )


    def __str__(self):

        return self.name
```

---

## 50.4 Form

```python
# products/forms.py

from django import forms

from .models import Product


class ProductForm(forms.ModelForm):

    class Meta:

        model = Product

        fields = [
            "name",
            "description",
            "price",
            "quantity",
        ]
```

---

## 50.5 Views

```python
# products/views.py

from django.contrib import messages
from django.db.models import Q
from django.shortcuts import get_object_or_404
from django.shortcuts import redirect
from django.shortcuts import render

from .forms import ProductForm
from .models import Product


def product_list(request):

    query = request.GET.get(
        "q",
        "",
    )

    products = Product.objects.all()

    if query:

        products = products.filter(
            Q(name__icontains=query)
            |
            Q(description__icontains=query)
        )

    return render(
        request,
        "products/list.html",
        {
            "products": products,
            "query": query,
        },
    )


def product_detail(
    request,
    product_id,
):

    product = get_object_or_404(
        Product,
        id=product_id,
    )

    return render(
        request,
        "products/detail.html",
        {
            "product": product,
        },
    )


def product_create(request):

    if request.method == "POST":

        form = ProductForm(
            request.POST,
        )

        if form.is_valid():

            form.save()

            messages.success(
                request,
                "Product created successfully.",
            )

            return redirect(
                "product-list",
            )

    else:

        form = ProductForm()

    return render(
        request,
        "products/create.html",
        {
            "form": form,
        },
    )


def product_delete(
    request,
    product_id,
):

    product = get_object_or_404(
        Product,
        id=product_id,
    )

    if request.method == "POST":

        product.delete()

        messages.success(
            request,
            "Product deleted successfully.",
        )

        return redirect(
            "product-list",
        )

    return render(
        request,
        "products/delete.html",
        {
            "product": product,
        },
    )
```

---

## 50.6 URLs

```python
# products/urls.py

from django.urls import path

from . import views


urlpatterns = [
    path(
        "",
        views.product_list,
        name="product-list",
    ),

    path(
        "create/",
        views.product_create,
        name="product-create",
    ),

    path(
        "<int:product_id>/",
        views.product_detail,
        name="product-detail",
    ),

    path(
        "<int:product_id>/delete/",
        views.product_delete,
        name="product-delete",
    ),
]
```

Main URL:

```python
# config/urls.py

from django.contrib import admin
from django.urls import include
from django.urls import path


urlpatterns = [
    path(
        "admin/",
        admin.site.urls,
    ),

    path(
        "products/",
        include("products.urls"),
    ),
]
```

---

## 50.7 Admin

```python
# products/admin.py

from django.contrib import admin

from .models import Product


@admin.register(Product)
class ProductAdmin(admin.ModelAdmin):

    list_display = (
        "id",
        "name",
        "price",
        "quantity",
        "created_at",
    )

    search_fields = (
        "name",
        "description",
    )

    list_filter = (
        "created_at",
    )

    ordering = (
        "-created_at",
    )
```

---

## 50.8 Base Template

```html
<!-- templates/base.html -->

{% load static %}

<!DOCTYPE html>

<html lang="en">

<head>

    <meta charset="UTF-8">

    <meta
        name="viewport"
        content="width=device-width, initial-scale=1.0"
    >

    <title>
        {% block title %}
            Django Shop
        {% endblock %}
    </title>

    <link
        rel="stylesheet"
        href="{% static 'css/style.css' %}"
    >

</head>

<body>

    <nav>

        <a
            href="{% url 'product-list' %}"
        >
            Products
        </a>

        <a
            href="{% url 'product-create' %}"
        >
            Create Product
        </a>

        <a href="/admin/">
            Admin
        </a>

    </nav>


    {% if messages %}

        {% for message in messages %}

            <div class="message">
                {{ message }}
            </div>

        {% endfor %}

    {% endif %}


    <main>

        {% block content %}
        {% endblock %}

    </main>

</body>

</html>
```

---

## 50.9 Product List

```html
<!-- templates/products/list.html -->

{% extends "base.html" %}

{% block title %}
Products
{% endblock %}


{% block content %}

<h1>
    Products
</h1>


<form method="get">

    <input
        type="search"
        name="q"
        value="{{ query }}"
        placeholder="Search products..."
    >

    <button type="submit">
        Search
    </button>

</form>


<hr>


{% for product in products %}

    <article>

        <h2>

            <a
                href="{% url 'product-detail' product.id %}"
            >
                {{ product.name }}
            </a>

        </h2>


        <p>
            {{ product.description }}
        </p>


        <p>
            Price:
            ${{ product.price }}
        </p>


        <p>
            Quantity:
            {{ product.quantity }}
        </p>

    </article>

{% empty %}

    <p>
        No products found.
    </p>

{% endfor %}


{% endblock %}
```

---

## 50.10 Product Detail

```html
<!-- templates/products/detail.html -->

{% extends "base.html" %}


{% block title %}
{{ product.name }}
{% endblock %}


{% block content %}

<h1>
    {{ product.name }}
</h1>


<p>
    {{ product.description }}
</p>


<p>
    Price:
    ${{ product.price }}
</p>


<p>
    Quantity:
    {{ product.quantity }}
</p>


<a
    href="{% url 'product-list' %}"
>
    Back
</a>


<a
    href="{% url 'product-delete' product.id %}"
>
    Delete
</a>

{% endblock %}
```

---

## 50.11 Create Product

```html
<!-- templates/products/create.html -->

{% extends "base.html" %}


{% block title %}
Create Product
{% endblock %}


{% block content %}

<h1>
    Create Product
</h1>


<form method="post">

    {% csrf_token %}

    {{ form.as_p }}


    <button type="submit">
        Create
    </button>

</form>

{% endblock %}
```

---

## 50.12 Delete Product

```html
<!-- templates/products/delete.html -->

{% extends "base.html" %}


{% block title %}
Delete Product
{% endblock %}


{% block content %}

<h1>
    Delete Product
</h1>


<p>
    Are you sure you want to delete
    "{{ product.name }}"?
</p>


<form method="post">

    {% csrf_token %}


    <button type="submit">
        Yes, Delete
    </button>


    <a
        href="{% url 'product-detail' product.id %}"
    >
        Cancel
    </a>

</form>

{% endblock %}
```

---

## 50.13 CSS

```css
/* static/css/style.css */

* {
    box-sizing: border-box;
}


body {
    font-family: Arial, sans-serif;

    max-width: 1000px;

    margin: 0 auto;

    padding: 30px;

    line-height: 1.6;
}


nav {
    display: flex;

    gap: 20px;

    margin-bottom: 30px;

    padding-bottom: 20px;

    border-bottom: 1px solid #ddd;
}


nav a {
    text-decoration: none;
}


article {
    padding: 20px;

    margin-bottom: 15px;

    border: 1px solid #ddd;

    border-radius: 8px;
}


input {
    padding: 10px;

    width: 300px;
}


button {
    padding: 10px 16px;

    cursor: pointer;
}


.message {
    padding: 12px;

    margin-bottom: 20px;

    border: 1px solid #ddd;

    border-radius: 6px;
}
```

---

## 50.14 Migrate

```bash
python manage.py makemigrations
```

```bash
python manage.py migrate
```

---

## 50.15 Create Admin User

```bash
python manage.py createsuperuser
```

---

## 50.16 Run

```bash
python manage.py runserver
```

Open:

```text
http://127.0.0.1:8000/products/
```

Admin:

```text
http://127.0.0.1:8000/admin/
```

---

# 51. GitHub Workflow

Initialize Git:

```bash
git init
```

Create `.gitignore`:

```gitignore
# Virtual environments
venv/
.venv/

# Python
__pycache__/
*.py[cod]

# Django
db.sqlite3
media/
staticfiles/

# Environment
.env

# IDE
.vscode/
.idea/

# OS
.DS_Store
Thumbs.db
```

Create requirements:

```bash
python -m pip freeze > requirements.txt
```

Add files:

```bash
git add .
```

Commit:

```bash
git commit -m "Initial Django project"
```

Rename branch:

```bash
git branch -M main
```

Add GitHub repository:

```bash
git remote add origin https://github.com/YOUR_USERNAME/YOUR_REPOSITORY.git
```

Push:

```bash
git push -u origin main
```

---

# 52. Learning Roadmap

## 🟢 Beginner

Learn in this order:

```text
Python
    ↓
HTTP Basics
    ↓
Django Installation
    ↓
Project
    ↓
App
    ↓
URLs
    ↓
Views
    ↓
Templates
    ↓
Static Files
    ↓
Models
    ↓
Migrations
    ↓
Admin
```

---

## 🟡 Intermediate

```text
Forms
    ↓
ModelForms
    ↓
Validation
    ↓
CRUD
    ↓
Authentication
    ↓
Authorization
    ↓
Sessions
    ↓
Messages
    ↓
Class-Based Views
    ↓
Generic Views
    ↓
Relationships
    ↓
File Uploads
    ↓
Pagination
    ↓
Search
    ↓
Transactions
```

---

## 🔴 Advanced

```text
Custom User Model
    ↓
Middleware
    ↓
Signals
    ↓
Async
    ↓
Caching
    ↓
Query Optimization
    ↓
Database Indexes
    ↓
Testing
    ↓
Security
    ↓
PostgreSQL
    ↓
Redis
    ↓
Background Tasks
    ↓
Docker
    ↓
CI/CD
    ↓
Production Deployment
```

---

# 53. Official Documentation

## Django

* [Django Official Website](https://www.djangoproject.com/)
* [Django 6.0 Documentation](https://docs.djangoproject.com/en/6.0/)
* [Django 6.0 Installation](https://docs.djangoproject.com/en/6.0/intro/install/)
* [Django Tutorial](https://docs.djangoproject.com/en/6.0/intro/tutorial01/)
* [Django Models](https://docs.djangoproject.com/en/6.0/topics/db/models/)
* [Django Queries](https://docs.djangoproject.com/en/6.0/topics/db/queries/)
* [Django Forms](https://docs.djangoproject.com/en/6.0/topics/forms/)
* [Django Authentication](https://docs.djangoproject.com/en/6.0/topics/auth/)
* [Django Sessions](https://docs.djangoproject.com/en/6.0/topics/http/sessions/)
* [Django Class-Based Views](https://docs.djangoproject.com/en/6.0/topics/class-based-views/)
* [Django File Uploads](https://docs.djangoproject.com/en/6.0/topics/http/file-uploads/)
* [Django Pagination](https://docs.djangoproject.com/en/6.0/topics/pagination/)
* [Django Middleware](https://docs.djangoproject.com/en/6.0/topics/http/middleware/)
* [Django Signals](https://docs.djangoproject.com/en/6.0/topics/signals/)
* [Django Async](https://docs.djangoproject.com/en/6.0/topics/async/)
* [Django Caching](https://docs.djangoproject.com/en/6.0/topics/cache/)
* [Django Testing](https://docs.djangoproject.com/en/6.0/topics/testing/)
* [Django Security](https://docs.djangoproject.com/en/6.0/topics/security/)
* [Django Deployment](https://docs.djangoproject.com/en/6.0/howto/deployment/)
* [Django Deployment Checklist](https://docs.djangoproject.com/en/6.0/howto/deployment/checklist/)
* [Django Static Files](https://docs.djangoproject.com/en/6.0/howto/static-files/)
* [Django PostgreSQL](https://docs.djangoproject.com/en/6.0/ref/databases/#postgresql-notes)

## Python

* [Python Official Website](https://www.python.org/)
* [Python Documentation](https://docs.python.org/3/)

## Git

* [Git Official Website](https://git-scm.com/)
* [Git Documentation](https://git-scm.com/doc)

## Django REST Framework

* [Django REST Framework](https://www.django-rest-framework.org/)

---

# 🎯 Final Goal

After learning this guide, you should be able to build:

```text
                    Django
                       │
          ┌────────────┼────────────┐
          │            │            │
          ▼            ▼            ▼
    Authentication   Products     Orders
          │            │            │
          └────────────┼────────────┘
                       │
                       ▼
                  PostgreSQL
                       │
                       ▼
                     Redis
                       │
                       ▼
                  REST API
                       │
                       ▼
                    Testing
                       │
                       ▼
                    Docker
                       │
                       ▼
                    CI/CD
                       │
                       ▼
                  Production
```

Possible projects:

```text
1. Blog
2. Authentication System
3. Product CRUD
4. Inventory System
5. Order Management
6. E-Commerce
7. REST API
8. Inventory + API
9. Dockerized Django
10. Production E-Commerce
```

---

# 🇰🇭 សេចក្តីសង្ខេប

Django គួររៀនតាមលំដាប់នេះ៖

```text
Python
 ↓
Django
 ↓
URL
 ↓
View
 ↓
Template
 ↓
Model
 ↓
ORM
 ↓
Database
 ↓
Forms
 ↓
CRUD
 ↓
Authentication
 ↓
Authorization
 ↓
API
 ↓
Testing
 ↓
Security
 ↓
PostgreSQL
 ↓
Redis
 ↓
Docker
 ↓
Deployment
```

**ចំណុចសំខាន់បំផុត** គឺត្រូវយល់ Request Flow៖

```text
Browser
   ↓
URL
   ↓
View
   ↓
Model / ORM
   ↓
Database
   ↓
View
   ↓
Template
   ↓
HTML Response
   ↓
Browser
```

បើអ្នកយល់ Flow នេះបានច្បាស់ អ្នកអាចចាប់ផ្តើមបង្កើត Django project ធំៗបាន។

---

# ⭐ Recommended Next Projects

```text
Project 1
Simple Blog
        ↓
Project 2
Login / Register
        ↓
Project 3
Product CRUD
        ↓
Project 4
Inventory Management
        ↓
Project 5
E-Commerce
        ↓
Project 6
Django REST API
        ↓
Project 7
Django + PostgreSQL
        ↓
Project 8
Django + PostgreSQL + Redis
        ↓
Project 9
Django + Docker
        ↓
Project 10
Full Production Application
```

---

## 📌 Version Note

This README targets **Django 6.0**.

Django 6.0 supports **Python 3.12, 3.13, and 3.14**. Always use documentation matching the Django version installed in your project.

Official Django documentation:

**https://docs.djangoproject.com/en/6.0/**

Django's own documentation provides the version-specific tutorials, API references, how-to guides, security documentation, testing documentation, and deployment documentation used throughout this README.
